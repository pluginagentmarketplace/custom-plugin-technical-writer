---
name: frontend-development
description: Master modern frontend development with React, Vue, Angular, and Next.js. Use this skill when learning about frontend frameworks, UI components, styling, or building interactive web applications.
---

# Frontend Development Skill

## Quick Start

### Essential Topics to Master
1. **HTML & CSS Foundation**
   - Semantic HTML structure
   - CSS Grid and Flexbox
   - Responsive design principles
   - CSS preprocessors (Sass, Less)

2. **JavaScript & DOM**
   - ES6+ features (arrow functions, destructuring, modules)
   - DOM manipulation and events
   - Async/await and Promises
   - Array and object methods

3. **React Framework**
   ```jsx
   import React, { useState } from 'react';

   function Counter() {
     const [count, setCount] = useState(0);
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={() => setCount(count + 1)}>Increment</button>
       </div>
     );
   }
   ```

4. **Component Patterns**
   - Functional components with hooks
   - Component composition
   - Props and state management
   - Context API for global state

5. **Modern Tooling**
   - Vite for fast development
   - ESLint for code quality
   - Prettier for code formatting
   - npm/yarn package management

## Framework Comparison

| Framework | Best For | Learning Curve | Community |
|-----------|----------|---|---|
| React | Large apps, reusable components | Medium | Excellent |
| Vue | Progressive enhancement, simplicity | Easy | Good |
| Angular | Enterprise applications | Hard | Good |
| Next.js | Full-stack React apps | Medium | Growing |

## Common Frontend Challenges

### State Management
- Prop drilling in deep component trees
- Complex state logic across components
- **Solutions**: Context API, Redux, Zustand

### Performance
- Large bundle sizes
- Unnecessary re-renders
- **Solutions**: Code splitting, lazy loading, memoization

### Accessibility
- WCAG compliance
- Screen reader support
- Keyboard navigation
- **Solutions**: Semantic HTML, ARIA labels, testing tools

## Testing Strategies

### Unit Testing
```javascript
import { render, screen } from '@testing-library/react';
import Counter from './Counter';

test('increments count', () => {
  render(<Counter />);
  const button = screen.getByRole('button');
  fireEvent.click(button);
  expect(screen.getByText(/Count: 1/)).toBeInTheDocument();
});
```

### Integration Testing
- Test component interactions
- Mock API calls
- Test user workflows

### E2E Testing
- Use Cypress or Playwright
- Test complete user journeys
- Cross-browser testing

## Performance Optimization

1. **Code Splitting**
   - Route-based splitting with React.lazy
   - Dynamic imports for heavy modules
   - Chunk optimization

2. **Rendering Optimization**
   - React.memo for pure components
   - useMemo for expensive calculations
   - useCallback for stable functions

3. **Bundle Analysis**
   - webpack-bundle-analyzer
   - Identify large dependencies
   - Tree shaking unused code

4. **Image Optimization**
   - Responsive images with srcset
   - WebP format with fallbacks
   - Lazy loading with Intersection Observer

## Build & Deployment

### Development Workflow
- Hot module replacement for fast feedback
- Source maps for debugging
- TypeScript for type safety

### Production Build
- Minification and compression
- Asset hashing for caching
- Environment-specific builds

### Deployment
- Static hosting (Vercel, Netlify, GitHub Pages)
- CDN distribution
- Cache headers configuration

## Best Practices

✅ **DO:**
- Use semantic HTML
- Implement proper error boundaries
- Test accessibility
- Optimize Core Web Vitals
- Use TypeScript for type safety
- Implement proper error handling
- Use environment variables for config

❌ **DON'T:**
- Ignore bundle size
- Forget mobile responsiveness
- Skip testing for edge cases
- Use `!important` excessively in CSS
- Hardcode configuration values
- Ignore console warnings and errors
- Skip accessibility considerations

## Resources

### Official Documentation
- React: https://react.dev
- Vue: https://vuejs.org
- Angular: https://angular.io
- Next.js: https://nextjs.org

### Learning Platforms
- FreeCodeCamp: Comprehensive tutorials
- Egghead.io: Advanced patterns
- Scrimba: Interactive learning
- Frontend Masters: Expert courses

## Next Steps
1. Choose your primary framework
2. Build small projects to practice
3. Contribute to open source
4. Study advanced patterns
5. Focus on accessibility and performance
