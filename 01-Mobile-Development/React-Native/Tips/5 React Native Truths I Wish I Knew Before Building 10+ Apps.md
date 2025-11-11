Source:
https://medium.com/@ssshubham660/5-react-native-truths-i-wish-i-knew-before-building-10-apps-9c1bc5de4198

# 1. “Global State” Isn’t Always the Answer (And Context Isn’t a Hero)

When I built my first app, I treated React’s Context API like duct tape. Authentication state? **Context**. Theme toggling? **Context**. A random counter? **Context**. By my third app, I’d created a dependency monster that re-rendered everything from the header to the footer if someone sneezed.

**What I wish I knew**:

- **Server state ≠ global state**: Tools like React Query or SWR handle data fetching, caching, and synchronization _way_ better than shoving everything into Redux or Context. Save global state for truly app-wide needs (e.g., user preferences).
- **Zustand over Context for small apps**: For lighter global state, Zustand’s API is simpler than Redux and avoids unnecessary re-renders.

# 2. Navigation Will Backstab You If You’re Not Careful

React Navigation is a gift… until it’s not. Early on, I’d structure my navigation containers haphazardly, only to spend hours debugging why the `goBack()`function behaved like a moody teenager.

**What I wish I knew**:

- **Structure your navigation tree early**: Use a root `NavigationContainer` and stick to a clear hierarchy (e.g., Stack -> Tabs -> Screens).
- **Type-check your navigators**: Use TypeScript or PropTypes to avoid passing invalid params. Trust me, “undefined is not an object” will haunt your dreams.
- **Deep linking isn’t optional**: Plan for it upfront. Your PM _will_ ask for it after launch.

# 3. Native Modules Aren’t Scary (But Avoid Them Until They Are)

I avoided native code like it was a legacy class component. Then I had to integrate a niche SDK, and my “expo install” bubble burst.

**What I wish I knew**:

- **Learn the basics of native modules early**: You don’t need to be a Swift/Java expert, but understanding how to bridge native code saves time.
- **Use** `**react-native-builder-bob**`: It automates scaffolding native modules, so you can focus on logic instead of Xcode tantrums.
- **Expo Modules API is your friend**: If using Expo, their new modules system simplifies native integrations.

# 4. Performance Is a Feature You Can’t Bolt On Later

My first app felt slower than a Windows update because I used `ScrollView`for a 1,000-item list. (Spoiler: The app crashed, and so did my confidence.)

**What I wish I knew**:

- `**FlatList**` **and** `**SectionList**` **are non-negotiable**: They recycle components like a thrift store. Use `getItemLayout` for fixed-height items to skip expensive measurements.
- **Memoize _strategically_**: Wrap components in `React.memo` and use `useCallback` for functions passed to child components. But don’t over-memoize—it’s not salt.
- **Profile with Flipper**: React Native’s Flipper plugin lets you inspect re-renders, network requests, and native logs. It’s like X-ray glasses for your app.

# 5. You’re Probably Over-Engineering

I once built a “scalable” folder structure with 10 nested layers for a to-do app. My teammate still brings it up at parties.

**What I wish I knew**:

- **Start with Expo**: Unless you _need_ custom native code, Expo gets you to MVP faster. Eject later if needed.
- **Boring code > clever code**: That fancy animated gradient button? It’ll break in Android 14. Use community libraries (check their GitHub issues first!).
- **Copy-paste is okay**: If you’re solving the same problem in three places, _then_ abstract it.