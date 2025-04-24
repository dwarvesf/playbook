# Versioning

Larger applications consists of multiple components that reference each other and rely on compatibility of the interfaces of the components.

To achieve the goal of loosely coupled applications, each component should be versioned independently hence allowing developers to detect breaking changes or seamless updates just by looking at the version number.

## Semantic Versioning

[**SemVer**](http://semver.org/) or **Semantic Versioning** is a way of versioning software in the format of **MAJOR.MINOR.PATCH**:

* MAJOR version when you make breaking changes,
* MINOR version when you add functionality in a backward-compatible manner for any specific MAJOR version, and
* PATCH version when you make backward-compatible bug fixes to any specific MAJOR.MINOR release.

This means that any time you release a new MAJOR release, the minor and patch values reset to 0, and the same is true of the patch value when making a MINOR release.

SemVer allows us to keep a consistent numbering scheme to ensure that going forward we can maintain compatibility with all system and platform components. It also allows us to upgrade dependencies with the confidence that things won’t break.

SemVer enables us to ensure that once something is committed, we can be confident that any version remains unique.

## FAQ

#### How should I deal with revisions in the 0.y.z initial development phase?

The simplest thing to do is start your initial development release at 0.1.0 and then increment the minor version for each subsequent release.

#### How do I know when to release 1.0.0?

If your software is being used in production, it should probably already be 1.0.0. If you have a stable API on which users have come to depend, you should be 1.0.0. If you’re worrying a lot about backward compatibility, you should probably already be 1.0.0.

#### How can I trust that things aren’t going to break with a minor or patch release update?

Like any practice, SemVer can be let down by people not using it correctly (like releasing a breaking change under a minor version rather than a major). It’s the responsibility of the person upgrading to ensure that the upgrade is appropriate and compatible.

#### The version number is getting big quickly

When a piece of software under heavy development many major/minor releases may happen in quick succession, so your version might start to look like 23.2.0 or 3.34.1. THAT’S OKAY! If that’s an accurate representation of what has happened in the code, then it’s appropriate that the version number reflects that.

What _might_ be a worry is if your version looked like 2.1.45 - did you release 45 patches/bug fixes in a row without adding any new features?
