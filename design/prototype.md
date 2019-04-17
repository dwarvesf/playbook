## Low-fidelity prototype: UI Design

### Research

#### 1. Understand prospect

Understanding stakeholder, company, business model, goals and challenges of the product in order to set mindset, define style, color palette, and typography, iconography, illustration, and photography.

#### 2. Define product

* Style

Based on the target user and product’s requirement, we define layout style. Make references from Dribbble, Behance and other showcase sites to catch up with the current design trends.

* Color

Define primary and secondary based on the logo and branding of the product. In case the client does not have a logo or verify their color palette, we can use Milanote app to create a moodboard that is a great way to set a visual direction from the client’s ideas.

Using 2 principles color to combine color in UI design:

    - 6:3:1 rule
    - Max 3 primary colors

Delivering a harmonious color scheme is clean and eye-friendly.

* Typography

Define typeface, font-size, font weight of each style (header, subheader, body text, etc.)

* Iconography

Define icon style based on elements’ style and branding characteristics. Icon color is defined following the color palette of the product.

* Illustration & Image

Illustration and image are used in accordance with content and style
You should apply a mask to bitmaps image when you export to an image file.

* Platform

Specify a particular screen (Desktop, Mobile or Tablet); divide into grid layout to align elements into columns, rows.

* Grid Layout

Grids are a framework that speeds up the designer-to-developer workflow by allowing developers to pre-set classes in their code that correspond to column sizes.

      - Select a suitable grid (we generally use 12 columns)
      - Use a baseline grid to align elements
      - Optimize grids for mobile and web app

* Responsive retrofitting

We live in a multiscreen-world. Everything needs to work across devices so responsive is a way to design a flexible screen. According to the requirement’s client or platform, we can decide on having a responsive or not.

### Demo Design

Design some demo screens and send to the client to verify style, color palette, typeface, font-size, etc. In case the client hasn’t decided on color palette yet, we will design without color first. Creating a screen in a grayscale color palette before adding color forces to focus layout, text style and spacing.

### Design System

#### What is Design System

Design systems enable teams to build better products faster by making design reusable—reusability makes scale possible. This is the heart and primary value of design systems. A design system is a collection of reusable components, guided by clear standards, that can be assembled together to build any number of applications.

![alt text](https://res.cloudinary.com/css-tricks/image/upload/c_scale,w_800,f_auto,q_auto/v1498084743/UXPin1_jkrrmb.png "Design System")

#### How we build Design System

##### Purpose and shared values

Before starting anything, it’s essential to align teams around a clear set of shared goals. It will help to build a vision and making sure everyone looks in the same direction. These goals will evolve with time and it’s normal. We just have to make sure that changes are broadly communicated.

##### Design principle

Design principles are the guiding sentences that help the teams to reach the purpose of the product thanks to the design. So you need to modify your practices and start establishing a style guide for the design system.

##### Color palette

Kick off your design system process with sprints devoted to unifying and implementing the color palette. Colors affect all the parts of the system, so you have to organize them first.

* Step 1: Create a moodboard
Using Milanote app to create a moodboard. Read more about how to make a moodboard
* Step 2: Identity primary and secondary colors
* Step 3: Send the color palette to customer
* Step 4: Decide on the naming convention
There are different approaches to naming colors in a design system. You can name colors using abstract names (e.g. #b9b9b9 - pigeon), actual names (e.g. #b9b9b9 - silver), numbers (e.g. #b9b9b9 - silver-1) or functional names (e.g. #b9b9b9 - silver-base)

* Step 5: Decide on the system of building accent palette colors
* Step 6: Test the color palette against the colors in the inventory
* Step 7: Implement new color palette in CSS (consider using a preprocessor and build a list of variables) on a test server
* Step 8: Test how the new palette affects the interface
* Step 9: Check the contrast between colors in the new UI. Make sure you comply with WCAG guidelines. 
Use the Contrast Ratio web app for quick access to WCAG color contrast ratios. You can read more the rules of contrast color to create “Contrast pairs” which clearly shows a WCAG tests.

* Step 10: Finalize the color palette
After tests and gathering feedback, finalize the palette and communicate it to the company. Add the palette to your design system documentation.

##### Typographic elements guide

Note preferred text sizes, spaces, fonts, etc. as well as any rules on where and when to use them. For example, how big are section headings or text body? Define details, like font weights, line heights, or custom kerning rules if applicable.

##### Graphic design assets

* Icons: All the icons that products, apps, or sites use. Having a standardized icon library ensures consistency across the entire brand.
* Photography: A single go-to reference for all product’s photography, both custom images, and purchased stock photos.
* Illustration: Compile all the custom illustrations commissioned, including page flourishes or border designs.
* Branding images: Standardized logos and other branding images, like mascots. Rules for logo usage can get strict, so it’s better to pull pre-approved images to ensure compliance.

##### Pattern library

List all design components with all states such as input, press, hover, etc.; categorize them by function, such as “navigation,” or by type, such as “drop-down menus.”

##### Tool

* Sketch
* Adobe XD

## High-fidelity Prototype: Interactive Design

### What

A high-fidelity prototype is an interaction-supported UI, which means a user can interact with it by triggering an action:
* Press a button
* Modify a slider in the filter section

Then, he or she can see or review how the prototype should react like a real product.

### When

It is created after we completed designing UI.

### Goal

* Test our assumptions for the product
* Quickly show our design concepts to the developers and customers
* Receive feedback for iteration

### Tools

* Principle
* Protopie
* Adobe XD
