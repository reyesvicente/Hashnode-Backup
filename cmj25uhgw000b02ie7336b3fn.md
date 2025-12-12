---
title: "Brutal Efficiency: A Tech Breakdown of My Portfolio"
datePublished: Fri Dec 12 2025 01:00:27 GMT+0000 (Coordinated Universal Time)
cuid: cmj25uhgw000b02ie7336b3fn
slug: brutal-efficiency-a-tech-breakdown-of-my-portfolio
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765421657652/6cb99bb6-1a35-4d93-9d99-e16bc7a2a3a1.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1765421682465/c486f078-86ba-4853-888f-1db0adbe40e7.png
tags: reactjs, typescript, showdev

---

This article provides a technical breakdown of how my 2026 portfolio website was built, covering the technology stack, design system, and animation techniques used to create its unique "Neobrutalist" aesthetic.

## **1\. The Tech Stack**

The project is built on a modern, high-performance React stack designed for speed and developer experience.

* **Core Framework**: [React 18](https://react.dev/)
    
* **Language**: [TypeScript](https://www.typescriptlang.org/) (for type safety and better developer tooling)
    
* **Build Tool**: [Vite](https://vitejs.dev/) (ensuring lightning-fast dev server startup and optimized builds)
    
* **Styling**: [Tailwind CSS v4](https://tailwindcss.com/) (using the new experimental v4 or latest v3 features for utility-first styling)
    
* **Icons**: [Lucide React](https://lucide.dev/) (clean, consistent SVG icons)
    

## **2\. Design System: Neobrutalism**

The visual style follows a **Neobrutalist** design trend. This is characterized by:

* **High Contrast**: Stark black borders (`border-2` or `border-4`) against vivid colors.
    
* **Bold Colors**: A custom color palette defined in the Tailwind configuration:
    
    * `neo-blue`
        
    * `neo-green`
        
    * `neo-yellow`
        
    * `neo-pink`
        
    * `neo-black` (likely a very dark gray/black)
        
    * `neo-offwhite`
        
* **Offset Shadows**: Elements often feature hard, non-blurred shadows (box-shadows with no blur radius) to give a "sticker" or "layered" paper look.
    
    * *Implementation Example*: A class like `shadow-neo` likely applies `box-shadow: 4px 4px 0px 0px #000;`.
        

## **3\. Animations & Interactivity**

The site feels alive due to a combination of animation libraries and CSS techniques.

### **Framer Motion**

Included via `framer-motion`, this library powers the complex entrance animations and state transitions.

* **Hero Section**: textual elements and the hero image slide in and fade up using `motion.div`.
    
    * *Code Snippet*:
        
        ```tsx
        <motion.div
          initial={{ opacity: 0, x: -50 }}
          animate={{ opacity: 1, x: 0 }}
          transition={{ duration: 0.8 }}
        >
        ```
        
* **Scroll Animations**: Elements reveal themselves as you scroll down the page, often triggered by `whileInView` or similar Framer Motion properties.
    

### **React CountUp & Intersection Observers**

The **Stats** section uses `react-countup` to animate numbers rolling up (e.g., "30+ Projects Completed") when they come into view.

* `react-intersection-observer` is used to detect when the user has scrolled to the numbers section, triggering the count-up animation only once (`triggerOnce: true`).
    

### **CSS Animations**

Standard CSS animations (configured via Tailwind) are used for continuous effects:

* **Pulse**: Background blobs in the Hero section use `animate-pulse` to gently fade in and out, creating a dynamic background.
    
* **Bounce**: Decorative elements (like the "HI!" badge) use `animate-bounce` to draw attention.
    
* **Hover Effects**: Buttons and cards have `transition-all` classes to snap or move slightly on hover, reinforcing the tactile feel of the UI.
    

## **4\. Key Components**

* **Hero.tsx**: The visual centerpiece. It layers `absolute` positioned background blobs, a grid layout for text vs image, and Framer Motion for the entrance.
    
* **Stats.tsx**: Demonstrates integration of third-party hooks (`useInView`) with animation libraries (`CountUp`) for a polished user experience.
    
* **ProjectModal.tsx**: Handles the detailed view of projects, likely using a fixed overlay with a high z-index and conditional rendering based on state.
    

## **Summary**

This portfolio combines the raw, bold aesthetic of Neobrutalism with the smooth, polished feel of modern React animations. The result is a site that looks retro but feels incredibly fast and responsive.