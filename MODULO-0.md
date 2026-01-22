# CSS Flexbox and Grid Layout Module

## Introduction
CSS layouts are essential in web development as they provide the structure needed for responsive and adaptive designs. Prior to integrating frameworks like Angular, understanding how to effectively use CSS for layout is crucial. This foundational knowledge allows developers to create visually appealing and functionally robust applications. CSS Flexbox and Grid Layout are powerful tools that enable this.

## Comprehensive Flexbox Section
### Flex Container Properties
- **display**: Defines a flex container; the inline or block version.
- **flex-direction**: Defines the direction flex items are placed in the flex container.
- **flex-wrap**: Specifies whether flex items are forced onto one line or can wrap onto multiple lines.
- **flex-flow**: A shorthand for `flex-direction` and `flex-wrap`.
- **justify-content**: Defines the alignment along the main axis (e.g., space-between, center).
- **align-items**: Aligns flex items along the cross axis (e.g., stretch, center).
- **align-content**: Aligns flex lines within the flex container.

### Flex Item Properties
- **flex-grow**: Defines the ability of a flex item to grow if necessary.
- **flex-shrink**: Defines the ability of a flex item to shrink if necessary.
- **flex-basis**: Defines the default size of an element before the remaining space is distributed.
- **align-self**: Allows the default alignment (or the one specified by `align-items`) to be overridden for individual flex items.

## Detailed CSS Grid Section
### Grid Template Areas
- Use grid template areas to define how cells are laid out visually.

```css
.container {
  display: grid;
  grid-template-areas:
    'header header'
    'main aside'
    'footer footer';
}
```

### Responsive Patterns
- Use media queries to adjust grid layouts for different screen sizes and orientations, enhancing usability across devices.

## Case Studies
1. **Navbar**: A responsive navigation bar using Flexbox to align items.
2. **App Layout**: A structured layout for a generic application using Grid.
3. **Dashboard**: A comprehensive dashboard layout utilizing both Flexbox and Grid for data visualization.

## Progressive Exercises
1. **Product Cards**: Create responsive product cards using Flexbox.
2. **Registration Form**: Design a registration form layout with a CSS Grid.
3. **Pinterest Gallery**: Build a Pinterest-style gallery using CSS Grid.
4. **Blog Layout**: Structure a blog page with Flexbox and Grid to display articles and side content.
5. **Analytics Dashboard**: Develop an analytics dashboard that adapts to screen sizes using a combination of both layout techniques.

## Real-World Angular Use Cases
- Implementing responsive designs in Angular applications.
- Utilizing Flexbox for responsive Angular components.
- Using CSS Grid for layout in Angular frontend development.

## Learning Resources
- [MDN Web Docs on CSS Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)
- [MDN Web Docs on CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
- [CSS-Tricks Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [CSS-Tricks Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)

## Mastery Checklist
- [ ] Understand Flexbox properties and usage.
- [ ] Master CSS Grid concepts.
- [ ] Create adaptive layouts for various screen sizes.

## Final Integrator Project
### Rick and Morty Landing Page
Create a landing page for the Rick and Morty series utilizing both Flexbox and Grid. The page should include:
- Responsive layout for mobile and desktop screens.
- Case studies showcased using real show details.
- Interactive elements that illustrate user engagement.