## Introduction

React Native 0.74 has been released in April of 2024. It's been a great release packed with over **1600 commits** of fixes and improvements!

Some of the highlights include:

- [Yoga 3.0](https://reactnative.dev/blog/2024/04/22/release-0.74#yoga-30)
- [New Architecture: Bridgeless by Default](https://reactnative.dev/blog/2024/04/22/release-0.74#new-architecture-bridgeless-by-default)
- [New Architecture: Batched `onLayout` Updates](https://reactnative.dev/blog/2024/04/22/release-0.74#new-architecture-batched-onlayout-updates)
- [Yarn 3 for New Projects](https://reactnative.dev/blog/2024/04/22/release-0.74#yarn-3-for-new-projects)

You can read the full release blog post here: [https://reactnative.dev/blog/2024/04/22/release-0.74](https://reactnative.dev/blog/2024/04/22/release-0.74)

## The new thing

While working on [React Native visionOS](https://github.com/callstack/react-native-visionos) I've added a new API that adds a hover effect when the user looks at an element. As it turns out, the same API is used on iPadOS to apply hover effects when using a mouse!

Later, I teamed up together with [Saad Najmi](https://twitter.com/SaadNajmi) from Microsoft to create a RFC which described this feature, you can check it [here](https://github.com/react-native-community/discussions-and-proposals/pull/750).

After the idea got validated in the RFC, Saad created a [PR](https://github.com/facebook/react-native/pull/43078) for it.

And that's how the new `cursor` style got introduced!

## Demo

If you add this style to any view it will have a hover effect on iPad:

```
<TouchableOpacity style={{ cursor: "pointer" }}>  	<Text>Click me</Text>  </TouchableOpacity>
```

_Note: It doesn't have to be a Touchable view._

Adding this feature to your app can greatly improve your users experience on iPad.