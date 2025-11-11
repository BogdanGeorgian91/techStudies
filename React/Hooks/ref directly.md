No more React.forwardRef boilerplate—refs can now be passed directly as props to functional components. Even better, you can expose specific functions to parent components using useImperativeHandle for precise control. 

In the example below, the MyInput component exposes only focus and clear to the parent via the ref, keeping the API clean and controlled. This is perfect for reusable components where you want to give parents just the right amount of control without overexposing internal refs.

The move to direct ref props in React Native 0.78 makes this pattern even smoother, cutting down on code while boosting clarity.

![[IMG_5719.jpeg]]