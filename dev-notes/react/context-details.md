# React Context

- [React](../react/index.md)

## Context is not optimized for high-frequency updates

React Context has some inherent performance characteristics that make it less suitable for high-frequency updates.

### Why Context isn't optimized for high-frequency updates:

When a Context value changes, React re-renders _all_ components that consume that context, even if they only use a small part of the context value. There's no built-in way to prevent these re-renders or make them more granular.

### Performance-critical scenarios where this becomes problematic:

1. **Real-time data streams** - Stock prices, cryptocurrency rates, or live sports scores updating multiple times per second
2. **Mouse/touch tracking** - Tracking cursor position or drag operations that fire dozens of events per second
3. **Animation state** - Managing animation progress or transition states that update on every frame (60fps)
4. **Form input state** - Large forms where every keystroke triggers context updates
5. **Gaming state** - Game loops updating player positions, scores, or other rapidly changing state

**Example scenario:**
Imagine a trading dashboard with 50 stock ticker components. If you put the stock prices in Context and they update 10 times per second, you'd have 500 component re-renders per second (50 components × 10 updates), even if each component only displays one stock price.

### Better alternatives for high-frequency updates:

- **State management libraries** like Zustand, Redux, or Jotai that offer subscription-based updates
- **Local component state** with props drilling for smaller apps
- **Custom hooks with refs** for values that don't need to trigger re-renders
- **External state managers** that components can subscribe to selectively

Context works great for relatively stable data like user authentication, theme settings, or configuration that changes infrequently.
