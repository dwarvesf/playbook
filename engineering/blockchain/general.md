### 1. Abstract-vs-interfaces

Be aware of the tradeoffs between abstract contracts and interfaces.
Both interfaces and abstract contracts provide one with a customizable and re-usable approach for smart contracts. Interfaces, which were introduced in Solidity 0.4.11, are similar to abstract contracts but cannot have any functions implemented. Interfaces also have limitations such as not being able to access storage or inherit from other interfaces which generally makes abstract contracts more practical. Although, interfaces are certainly useful for designing contracts prior to implementation. Additionally, it is important to keep in mind that if a contract inherits from an abstract contract it must implement all non-implemented functions via overriding or it will be abstract as well.

### 2. assert require revert
#### Enforce invariants with `assert()`

An assert guard triggers when an assertion fails - such as an invariant property changing. For
example, the token to ether issuance ratio, in a token issuance contract, may be fixed. You can
verify that this is the case at all times with an `assert()`. Assert guards should often be
combined with other techniques, such as pausing the contract and allowing upgrades. (Otherwise, you
may end up stuck, with an assertion that is always failing.)

Example:

```sol
contract Token {
    mapping(address => uint) public balanceOf;
    uint public totalSupply;

    function deposit() public payable {
        balanceOf[msg.sender] += msg.value;
        totalSupply += msg.value;
        assert(address(this).balance >= totalSupply);
    }
}
```

Note that the assertion is *not* a strict equality of the balance because the contract can be
[forcibly sent ether](#remember-that-ether-can-be-forcibly-sent-to-an-account) without going
through the `deposit()` function!


#### Use `assert()`, `require()`, `revert()` properly

!!! Info
    The convenience functions **assert** and **require** can be used to check for conditions and throw an exception if the condition is not met.

The **assert** function should only be used to test for internal errors, and to check invariants.

The **require** function should be used to ensure valid conditions, such as inputs, or contract state variables are met, or to validate return values from calls to external contracts. <sup><a href='https://solidity.readthedocs.io/en/latest/control-structures.html#error-handling-assert-require-revert-and-exceptions'>\*</a></sup>

Following this paradigm allows formal analysis tools to verify that the invalid opcode can never be
reached: meaning no invariants in the code are violated and that the code is formally verified.

```sol
pragma solidity ^0.5.0;

contract Sharer {
    function sendHalf(address payable addr) public payable returns (uint balance) {
        require(msg.value % 2 == 0, "Even value required."); //Require() can have an optional message string
        uint balanceBeforeTransfer = address(this).balance;
        (bool success, ) = addr.call.value(msg.value / 2)("");
        require(success);
        // Since we reverted if the transfer failed, there should be
        // no way for us to still have half of the money.
        assert(address(this).balance == balanceBeforeTransfer - msg.value / 2); // used for internal error checking
        return address(this).balance;
    }
}
```

See [SWC-110](https://swcregistry.io/docs/SWC-110) & [SWC-123](https://swcregistry.io/docs/SWC-123)

## 3.Complex inheritance
When utilizing multiple inheritance in Solidity, it is important to understand how the compiler
composes the inheritance graph.

```sol

contract Final {
    uint public a;
    function Final(uint f) public {
        a = f;
    }
}

contract B is Final {
    int public fee;

    function B(uint f) Final(f) public {
    }
    function setFee() public {
        fee = 3;
    }
}

contract C is Final {
    int public fee;

    function C(uint f) Final(f) public {
    }
    function setFee() public {
        fee = 5;
    }
}

contract A is B, C {
  function A() public B(3) C(5) {
      setFee();
  }
}
```

When a contract is deployed, the compiler will *linearize* the inheritance from right to left
(after the keyword _is_ the parents are listed from the most base-like to the most derived). Here
is contract A's linearization:

**Final \<- B \<- C \<- A**

The consequence of the linearization will yield a `fee` value of 5, since C is the most derived
contract. This may seem obvious, but imagine scenarios where C is able to shadow crucial functions,
reorder boolean clauses, and cause the developer to write exploitable contracts. Static analysis
currently does not raise issue with overshadowed functions, so it must be manually inspected.

For more on security and inheritance, check out this
[article](https://pdaian.com/blog/solidity-anti-patterns-fun-with-inheritance-dag-abuse/)

To help contribute, Solidity's Github has a
[project](https://github.com/ethereum/solidity/projects/9#card-8027020) with all
inheritance-related issues.

See [SWC-125](https://swcregistry.io/docs/SWC-125)

## 4. Event monitoring

It can be useful to have a way to monitor the contract's activity after it was deployed. One way to
accomplish this is to look at all transactions of the contract, however that may be insufficient,
as message calls between contracts are not recorded in the blockchain. Moreover, it shows only the
input parameters, not the actual changes being made to the state. Also, events could be used to
trigger functions in the user interface.

```sol
contract Charity {
    mapping(address => uint) balances;

    function donate() payable public {
        balances[msg.sender] += msg.value;
    }
}

contract Game {
    function buyCoins() payable public {
        // 5% goes to charity
        charity.donate.value(msg.value / 20)();
    }
}
```

Here, `Game` contract will make an internal call to `Charity.donate()`. This transaction won't
appear in the external transaction list of `Charity`, but only visible in the internal
transactions.

An event is a convenient way to log something that happened in the contract. Events that were
emitted stay in the blockchain along with the other contract data and they are available for future
audit. Here is an improvement to the example above, using events to provide a history of the
Charity's donations.

```sol
contract Charity {
    // define event
    event LogDonate(uint _amount);

    mapping(address => uint) balances;

    function donate() payable public {
        balances[msg.sender] += msg.value;
        // emit event
        emit LogDonate(msg.value);
    }
}

contract Game {
    function buyCoins() payable public {
        // 5% goes to charity
        charity.donate.value(msg.value / 20)();
    }
}

```

Here, all transactions that go through the `Charity` contract, either directly or not, will show up
in the event list of that contract along with the amount of donated money.

______________________________________________________________________

!!! Note "Prefer newer Solidity constructs"
    Prefer constructs/aliases such as `selfdestruct` (over `suicide`) and `keccak256` (over `sha3`).  Patterns like `require(msg.sender.send(1 ether))` can also be simplified to using `transfer()`, as in `msg.sender.transfer(1 ether)`. Check out [Solidity Change log](https://github.com/ethereum/solidity/blob/develop/Changelog.md) for more similar changes.

## 5. Extcodesize checks
Avoid using `extcodesize` to check for Externally Owned Accounts.

The following modifier (or a similar check) is often used to verify whether a call was made from an
externally owned account (EOA) or a contract account:

```sol
// bad
modifier isNotContract(address _a) {
  uint size;
  assembly {
    size := extcodesize(_a)
  }
    require(size == 0);
     _;
}
```

The idea is straightforward: if an address contains code, it's not an EOA but a contract account.
However, **a contract does not have source code available during construction**. This means that
while the constructor is running, it can make calls to other contracts, but `extcodesize` for its
address returns zero. Below is a minimal example that shows how this check can be circumvented:

```sol
contract OnlyForEOA {    
    uint public flag;

    // bad
    modifier isNotContract(address _a){
        uint len;
        assembly { len := extcodesize(_a) }
        require(len == 0);
        _;
    }

    function setFlag(uint i) public isNotContract(msg.sender){
        flag = i;
    }
}

contract FakeEOA {
    constructor(address _a) public {
        OnlyForEOA c = OnlyForEOA(_a);
        c.setFlag(1);
    }
}
```

Because contract addresses can be pre-computed, this check could also fail if it checks an address
which is empty at block `n`, but which has a contract deployed to it at some block greater than
`n`.

!!! Warning "This issue is nuanced."
    If your goal is to prevent other contracts from being able to call your contract, the `extcodesize` check is probably sufficient. An alternative approach is to check the value of `(tx.origin == msg.sender)`, though this also has drawbacks.

    There may be other situations in which the `extcodesize` check serves your purpose. Describing all of them here is out of scope. Understand the underlying behaviors of the EVM and use your Judgement.

## 6. Fallback functions
#### Keep fallback functions simple

[Fallback functions](http://solidity.readthedocs.io/en/latest/contracts.html#fallback-function) are
called when a contract is sent a message with no arguments (or when no function matches), and only
has access to 2,300 gas when called from a `.send()` or `.transfer()`. If you wish to be able to
receive Ether from a `.send()` or `.transfer()`, the most you can do in a fallback function is log
an event. Use a proper function if a computation of more gas is required.

```sol
// bad
function() payable { balances[msg.sender] += msg.value; }

// good
function deposit() payable external { balances[msg.sender] += msg.value; }

function() payable { require(msg.data.length == 0); emit LogDepositReceived(msg.sender); }
```

#### Check data length in fallback functions

Since the
[fallback functions](http://solidity.readthedocs.io/en/latest/contracts.html#fallback-function) is
not only called for plain ether transfers (without data) but also when no other function matches,
you should check that the data is empty if the fallback function is intended to be used only for
the purpose of logging received Ether. Otherwise, callers will not notice if your contract is used
incorrectly and functions that do not exist are called.

```sol
// bad
function() payable { emit LogDepositReceived(msg.sender); }

// good
function() payable { require(msg.data.length == 0); emit LogDepositReceived(msg.sender); }
```

## 7. Integer Division
All integer division rounds down to the nearest integer. If you need more precision, consider using
a multiplier, or store both the numerator and denominator.

(In the future, Solidity will have a
[fixed-point](https://solidity.readthedocs.io/en/develop/types.html#fixed-point-numbers) type,
which will make this easier.)

```sol
// bad
uint x = 5 / 2; // Result is 2, all integer division rounds DOWN to the nearest integer
```

Using a multiplier prevents rounding down, this multiplier needs to be accounted for when working
with x in the future:

```sol
// good
uint multiplier = 10;
uint x = (5 * multiplier) / 2;
```

Storing the numerator and denominator means you can calculate the result of `numerator/denominator`
off-chain:

```sol
// good
uint numerator = 5;
uint denominator = 2;
```

## 8. interface types
When a function takes a contract address as an argument, it is better to pass an interface or
contract type rather than a raw `address`. If the function is called elsewhere within the source
code, the compiler will provide additional type safety guarantees.

Here we see two alternatives:

```sol
contract Validator {
    function validate(uint) external returns(bool);
}

contract TypeSafeAuction {
    // good
    function validateBet(Validator _validator, uint _value) internal returns(bool) {
        bool valid = _validator.validate(_value);
        return valid;
    }
}

contract TypeUnsafeAuction {
    // bad
    function validateBet(address _addr, uint _value) internal returns(bool) {
        Validator validator = Validator(_addr);
        bool valid = validator.validate(_value);
        return valid;
    }
}
```

The benefits of using the `TypeSafeAuction` contract above can then be seen from the following
example. If `validateBet()` is called with an `address` argument, or a contract type other than
`Validator`, the compiler will throw this error:

```sol
contract NonValidator{}

contract Auction is TypeSafeAuction {
    NonValidator nonValidator;

    function bet(uint _value) {
        bool valid = validateBet(nonValidator, _value); // TypeError: Invalid type for argument in function call.
                                                        // Invalid implicit conversion from contract NonValidator
                                                        // to contract Validator requested.
    }
}
```

## 9. Locking pragmas
Contracts should be deployed with the same compiler version and flags that they have been tested
the most with. Locking the pragma helps ensure that contracts do not accidentally get deployed
using, for example, the latest compiler which may have higher risks of undiscovered bugs. Contracts
may also be deployed by others and the pragma indicates the compiler version intended by the
original authors.

```sol
// bad
pragma solidity ^0.4.4;


// good
pragma solidity 0.4.4;
```

Note: a floating pragma version (ie. `^0.4.25`) will compile fine with `0.4.26-nightly.2018.9.25`,
however nightly builds should never be used to compile code for production.

!!! Warning
    Pragma statements can be allowed to float when a contract is intended for consumption
    by other developers, as in the case with contracts in a library or EthPM package. Otherwise, the
    developer would need to manually update the pragma in order to compile locally.

    See [SWC-103](https://swcregistry.io/docs/SWC-103)

## 10. Modifier
The code inside a modifier is usually executed before the function body, so any state changes or
external calls will violate the
[Checks-Effects-Interactions](https://solidity.readthedocs.io/en/develop/security-considerations.html#use-the-checks-effects-interactions-pattern)
pattern. Moreover, these statements may also remain unnoticed by the developer, as the code for
modifier may be far from the function declaration. For example, an external call in modifier can
lead to the reentrancy attack:

```sol
contract Registry {
    address owner;

    function isVoter(address _addr) external returns(bool) {
        // Code
    }
}

contract Election {
    Registry registry;

    modifier isEligible(address _addr) {
        require(registry.isVoter(_addr));
        _;
    }

    function vote() isEligible(msg.sender) public {
        // Code
    }
}
```

In this case, the `Registry` contract can make a reentrancy attack by calling `Election.vote()`
inside `isVoter()`.

!!! Note
    Use [modifiers](https://solidity.readthedocs.io/en/develop/contracts.html#function-modifiers) to
    replace duplicate condition checks in multiple functions, such as `isOwner()`, otherwise use
    `require` or `revert` inside the function. This makes your smart contract code more readable and
    easier to audit.

## 11. payability
Starting from Solidity `0.4.0`, every function that is receiving ether must use `payable` modifier,
otherwise if the transaction has `msg.value > 0` will revert
(except when forced).

!!! Note
    Something that might not be obvious: The `payable` modifier only applies to calls from *external* contracts. If I call a non-payable function in the payable function in the same contract, the non-payable function won't fail, though `msg.value` is still set.

## 12. Shadowing
It is currently possible to [shadow](https://en.wikipedia.org/wiki/Variable_shadowing) built-in
globals in Solidity. This allows contracts to override the functionality of built-ins such as `msg`
and `revert()`. Although this [is intended](https://github.com/ethereum/solidity/issues/1249), it
can mislead users of a contract as to the contract's true behavior.

```sol
contract PretendingToRevert {
    function revert() internal constant {}
}

contract ExampleContract is PretendingToRevert {
    function somethingBad() public {
        revert();
    }
}
```

Contract users (and auditors) should be aware of the full smart contract source code of any
application they intend to use.

## 13. timestamp-dependence
There are three main considerations when using a timestamp to execute a critical function in a
contract, especially when actions involve fund transfer.

#### Timestamp Manipulation

Be aware that the timestamp of the block can be manipulated by a miner. Consider this
[contract](https://etherscan.io/address/0xcac337492149bdb66b088bf5914bedfbf78ccc18#code):

```sol

uint256 constant private salt =  block.timestamp;

function random(uint Max) constant private returns (uint256 result){
    //get the best seed for randomness
    uint256 x = salt * 100/Max;
    uint256 y = salt * block.number/(salt % 5) ;
    uint256 seed = block.number/3 + (salt % 300) + Last_Payout + y;
    uint256 h = uint256(block.blockhash(seed));

    return uint256((h / x)) % Max + 1; //random number between 1 and Max
}
```

When the contract uses the timestamp to seed a random number, the miner can actually post a
timestamp within 15 seconds of the block being validated, effectively allowing the miner to
precompute an option more favorable to their chances in the lottery. Timestamps are not random and
should not be used in that context.

#### The 15-second Rule

The [Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf) (Ethereum's reference
specification) does not specify a constraint on how much blocks can drift in time, but
[it does specify](https://ethereum.stackexchange.com/a/5926/46821) that each timestamp should be
bigger than the timestamp of its parent. Popular Ethereum protocol implementations
[Geth](https://github.com/ethereum/go-ethereum/blob/4e474c74dc2ac1d26b339c32064d0bac98775e77/consensus/ethash/consensus.go#L45)
and
[Parity](https://github.com/paritytech/parity-ethereum/blob/73db5dda8c0109bb6bc1392624875078f973be14/ethcore/src/verification/verification.rs#L296-L307)
both reject blocks with timestamp more than 15 seconds in future. Therefore, a good rule of thumb
in evaluating timestamp usage is:

!!! Note
    If the scale of your time-dependent event can vary by 15 seconds and maintain integrity,
    it is safe to use a `block.timestamp`.

#### Avoid using `block.number` as a timestamp

It is possible to estimate a time delta using the `block.number` property and
[average block time](https://etherscan.io/chart/blocktime), however this is not future proof as
block times may change (such as
[fork reorganisations](https://blog.ethereum.org/2015/08/08/chain-reorganisation-depth-expectations/)
and the [difficulty bomb](https://github.com/ethereum/EIPs/issues/649)). In a sale spanning days,
the 15-second rule allows one to achieve a more reliable estimate of time.

See [SWC-116](https://swcregistry.io/docs/SWC-116)

## 14. tx-origin

Never use `tx.origin` for authorization, another contract can have a method which will call your
contract (where the user has some funds for instance) and your contract will authorize that
transaction as your address is in `tx.origin`.

```sol
contract MyContract {

    address owner;

    function MyContract() public {
        owner = msg.sender;
    }

    function sendTo(address receiver, uint amount) public {
        require(tx.origin == owner);
        (bool success, ) = receiver.call.value(amount)("");
        require(success);
    }

}

contract AttackingContract {

    MyContract myContract;
    address attacker;

    function AttackingContract(address myContractAddress) public {
        myContract = MyContract(myContractAddress);
        attacker = msg.sender;
    }

    function() public {
        myContract.sendTo(attacker, msg.sender.balance);
    }

}
```

You should use `msg.sender` for authorization (if another contract calls your contract `msg.sender`
will be the address of the contract and not the address of the user who called the contract).

You can read more about it here:
[Solidity docs](https://solidity.readthedocs.io/en/develop/security-considerations.html#tx-origin)

!!! Warning
    Besides the issue with authorization, there is a chance that `tx.origin` will be
    removed from the Ethereum protocol in the future, so code that uses `tx.origin` won't be compatible
    with future releases
    [Vitalik: 'Do NOT assume that tx.origin will continue to be usable or meaningful.'](https://ethereum.stackexchange.com/questions/196/how-do-i-make-my-dapp-serenity-proof/200#200)

    It's also worth mentioning that by using `tx.origin` you're limiting interoperability between
    contracts because the contract that uses tx.origin cannot be used by another contract as a contract
    can't be the `tx.origin`.

    See [SWC-115](https://swcregistry.io/docs/SWC-115)

## 15. Visibility
Explicitly label the visibility of functions and state variables. Functions can be specified as
being `external`, `public`, `internal` or `private`. Please understand the differences between
them, for example, `external` may be sufficient instead of `public`. For state variables,
`external` is not possible. Labeling the visibility explicitly will make it easier to catch
incorrect assumptions about who can call the function or access the variable.

- `External` functions are part of the contract interface. An external function `f` cannot be
  called internally (i.e. `f()` does not work, but `this.f()` works). External functions are
  sometimes more efficient when they receive large arrays of data.
- `Public` functions are part of the contract interface and can be either called internally or via
  messages. For public state variables, an automatic getter function (see below) is generated.
- `Internal` functions and state variables can only be accessed internally, without using `this`.
- `Private` functions and state variables are only visible for the contract they are defined in and
  not in derived contracts. **Note**: Everything that is inside a contract is visible to all
  observers external to the blockchain, even `Private`
  variables.<sup><a href='https://solidity.readthedocs.io/en/develop/contracts.html?#visibility-and-getters'>\*</a></sup>

```sol
// bad
uint x; // the default is internal for state variables, but it should be made explicit
function buy() { // the default is public
    // public code
}

// good
uint private y;
function buy() external {
    // only callable externally or using this.buy()
}

function utility() public {
    // callable externally, as well as internally: changing this code requires thinking about both cases.
}

function internalAction() internal {
    // internal code
}
```

### See 

- [SWC-100](https://swcregistry.io/docs/SWC-100) and
- [SWC-108](https://swcregistry.io/docs/SWC-108)
