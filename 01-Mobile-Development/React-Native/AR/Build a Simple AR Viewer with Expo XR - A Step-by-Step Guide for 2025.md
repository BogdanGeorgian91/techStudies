https://medium.com/@ssshubham660/build-a-simple-ar-viewer-with-expo-xr-a-step-by-step-guide-for-2025-81e998b01ede

**What You’ll Create**:  
An AR app that places a 3D cube in your real-world environment using React Native and Expo XR. No prior AR experience needed!

# Tools You’ll Need

- **Expo SDK 50+** (supports XR plugins)
- **A physical iOS/Android device** (ARCore/ARKit required)
- **Basic React Native knowledge**
# Step 1: Set Up Your Project

# Create a new Expo project  
npx create-expo-app ARViewerDemo  
cd ARViewerDemo

# Install dependencies  
npx expo install expo-gl expo-three three @expo/vector-icons expo-sensors  
npx expo install expo-xr  # Expo's AR/VR module

# Step 2: Configure app.json

Enable ARKit (iOS) and ARCore (Android):

{  
  "expo": {  
    "plugins": [  
      [  
        "expo-xr",  
        {  
          "cameraPermission": "Allow ARViewer to access your camera."  
        }  
      ]  
    ],  
    "ios": {  
      "infoPlist": {  
        "NSCameraUsageDescription": "AR experiences require camera access."  
      }  
    }  
  }  
}

# Step 3: Build the AR Scene

Replace **App.js** with:

import React from 'react';  
import { View, StyleSheet } from 'react-native';  
import { GLView } from 'expo-gl';  
import { Renderer } from 'expo-three';  
import { AR } from 'expo-xr';  
import * as THREE from 'three';  
  
export default function App() {  
  let cube;  
  // Initialize AR scene  
  const onContextCreate = async (gl) => {  
    const renderer = new Renderer({ gl });  
    renderer.setSize(gl.drawingBufferWidth, gl.drawingBufferHeight);  
    // Setup scene, camera, and AR session  
    const scene = new THREE.Scene();  
    const camera = new THREE.PerspectiveCamera(75, gl.drawingBufferWidth / gl.drawingBufferHeight, 0.1, 1000);  
      
    // Start AR  
    await AR.startAsync();  
    AR.setShouldAttemptRelocalization(true); // For persistent AR  
    // Add lighting  
    const light = new THREE.DirectionalLight(0xffffff, 1);  
    light.position.set(1, 1, 1);  
    scene.add(light);  
    // Create a red cube  
    const geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);  
    const material = new THREE.MeshStandardMaterial({ color: 0xff0000 });  
    cube = new THREE.Mesh(geometry, material);  
    cube.position.set(0, 0, -0.5); // 0.5 meters in front of camera  
    scene.add(cube);  
    // Render loop  
    const render = () => {  
      requestAnimationFrame(render);  
        
      // Update cube rotation  
      cube.rotation.x += 0.01;  
      cube.rotation.y += 0.01;  
      // Sync AR camera with device motion  
      if (AR.getCurrentFrame()) {  
        const { transform } = AR.getCurrentFrame().camera;  
        camera.matrix.fromArray(transform);  
        camera.matrix.decompose(camera.position, camera.quaternion, camera.scale);  
      }  
      renderer.render(scene, camera);  
      gl.endFrameEXP();  
    };  
    render();  
  };  
  return (  
    <View style={styles.container}>  
      <GLView  
        style={styles.arContainer}  
        onContextCreate={onContextCreate}  
      />  
    </View>  
  );  
}  
const styles = StyleSheet.create({  
  container: { flex: 1 },  
  arContainer: { flex: 1 }  
});

# Step 4: Test Your AR Viewer

1. **On iOS**:

npx expo run:ios --device

1. **On Android**:

npx expo run:android

**What to expect**:

- Point your camera at a flat surface.
- A rotating red cube appears floating in space!

# Step 5: Enhance It! (Optional)

## Add Tap-to-Place Functionality

// Add to imports  
import { TapGesture } from 'react-native-gesture-handler';  
  
// Inside component  
<TapGesture  
  numberOfTaps={1}  
  onActivated={({ nativeEvent }) => {  
    if (cube) {  
      // Convert screen tap to AR coordinates  
      const hit = AR.hitTest(nativeEvent.absoluteX, nativeEvent.absoluteY);  
      if (hit) {  
        cube.position.copy(hit.position);  
      }  
    }  
  }}  
>  
  <GLView ... />  
</TapGesture>

## Load a 3D Model Instead

import { OBJLoader } from 'three/examples/jsm/loaders/OBJLoader';\  
  
// Replace cube setup with:  
const loader = new OBJLoader();  
loader.load(  
  require('./assets/model.obj'),  
  (object) => {  
    object.scale.set(0.1, 0.1, 0.1);  
    scene.add(object);  
  }  
);

# Troubleshooting

- **Black Screen?** Ensure ARCore/ARKit is [installed](https://developers.google.com/ar/discover/supported-devices) on your device.
- **Jittery Motion?** Add `AR.setWorldAlignment('gravity')` for stable tracking.
# Why This Works

- **Expo XR** handles device-specific AR APIs (ARKit/ARCore) under the hood.
- **Three.js** simplifies 3D math and rendering.
- **React Native** manages the UI thread efficiently.

**Next Steps**:

1. Experiment with textures (`THREE.TextureLoader`).
2. Add shadows using `renderer.shadowMap.enabled = true`.
3. Explore [ViroReact](https://viromedia.com/) for advanced interactions.