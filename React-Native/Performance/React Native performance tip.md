
```
Your React Native app can get slower because of one tiny mistake… hiding right in your JSX.

⚠️ The silent killer: Inline functions inside JSX
Looks harmless?

<Pressable onPress={() => handlePress(item.id)}>
 <Text>{item.name}</Text>
</Pressable>

But every single render, React creates a new function in memory.
React thinks:
"New function reference? Must re-render!"

And then:
❌ Unnecessary re-renders
❌ Broken FlatList recycling
❌ Choppy animations

🤔 "But… I used useCallback!"
Truth bomb 💣 —
If your useCallback has dynamic dependencies, it still recreates functions on every render.
Same problem, just hidden behind a hook.

✅ What actually works:

1️⃣ Pre-bind handlers:
const handlePressWithId = (id) => () => handlePress(id);

<Pressable onPress={handlePressWithId(item.id)}>
 <Text>{item.name}</Text>
</Pressable>

👉 Why does this help?
 Because handlePressWithId(item.id) only returns a new function when the id changes. If your list item doesn’t change, it sees the same function reference and skips re-renders.

2️⃣ Smart memoization:
const onPressItem = useCallback((id) => {
 // Your logic here
}, []);  

const renderItem = useCallback(({ item }) => (
 <Pressable onPress={() => onPressItem(item.id)}>
  <Text>{item.name}</Text>
 </Pressable>
), [onPressItem]);

👉 Why does this help?
By giving it a memoized renderItem and stable callback references, React Native knows each item is the same and can reuse it. This keeps your scroll super smooth and reduces unnecessary work.

My personal checklist before blaming React Native:
✅ No inline callbacks in JSX
✅ Stable keys in lists
✅ useCallback with only stable dependencies

Once you fix these:
✨ Smooth animations
✨ Buttery scroll
✨ JS thread stays cool
```