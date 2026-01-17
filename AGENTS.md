Based on the concept of the high-end watch you presented, we have created a universal design document for "3D Product Storytelling" that can be applied to other high-value-added products. This design aims to progressively increase user purchasing intent by sequentially showcasing the product's "Appearance," "Refinement," "Innovation," and "Diversity."


High-End 3D Product Experience: Universal Design Parameters


1. Timeline - Configuration Table
We assume the total scrollable length is 500vh (1 section = 100vh) and design based on the progress (Scroll %).


| Section Range (vh) | Objective of Presentation | Key Shader |
|---|---|---|
| 1. Appearance: 0 - 100 | Mysterious appearance & sense of anticipation | Fresnel, Hologram |
| 2. Detail: 100 - 200 | Emphasis on craftsmanship | Metallic (PBR), Parallax |
| 3. Innovation: 200 - 300 | Technical superiority & structural beauty | Displacement |
| 4. Variation: 300 - 400 | Personalization & selection | PBR (Material Switching) |
| 5. Conclusion: 400 - 500 | Landing in the brand's world | Background Noise, Post-process |


2. Section-Specific Detailed Parameters


Section 1: Hero (Appearance)
- Camera: FOV 35° / Pos: $(0, 0, 10)$ → $(0, 0, 5)$
- Object Anim: Rotation $(0, -1.5, 0)$ → $(0, 0.5, 0)$, Scale $0.8$ → $1.0$
- Lighting: Environment Intensity $0$ → $1.0$, SpotLight $(5, 5, 5)$ Intensity $0$ → $50$
- Shader Uniforms: uFresnelPower: $4.0 \to 1.5$, uHologramMix: $1.0 \to 0.0$
- Easing: easeOutQuart


Section 2: Detail Zoom
- Camera: FOV 25° / Pos: $(0, 0, 5)$ → $(0.5, 0.2, 1.5)$ / Target: $(0.5, 0.2, 0)$
- Object Anim: Rotation $(0, 0.5, 0)$ → $(-0.2, 0.8, 0.1)$
- Lighting: PointLight $(1, 1, 1)$ follows mouse to emphasize glossiness
- Shader Uniforms: uRoughness: $0.1$ (fixed), uNormalScale: $1.0 \to 1.5$
- Easing: easeInOutCubic


Section 3: Mechanism (Exploded View)
- Camera: FOV 45° / Pos: $(0, 0, 4)$ (pull back shot)
- Object Anim: Parts spread out in the Z-axis direction ($0 \to 2.5$), Rotation on each axis is varied.
- Shader Uniforms: uExplodeProgress: $0 \to 1.0$, uGlowIntensity: $0 \to 2.0$, uDisplacement: $0.05$
- Easing: easeOutElastic(1, 0.75) (bouncy movement of parts)


Section 4: Material Experience
- Camera: FOV 35° / Pos: Fixed $(0, 0, 3)$
- Object Anim: Slow automatic rotation of 360 degrees
- Shader Uniforms: uMaterialMix changes from $0 \to 1 \to 2$ according to scroll amount, crossfading textures.
- Materials:
    * Mat A: Metalness: 1.0, Roughness: 0.1 (Chrome)
    * Mat B: Metalness: 0.0, Roughness: 0.8 (Carbon/Matte)
- Easing: linear (equal speed for material changes)


Section 5: Ending
- Camera: FOV 35° / Pos: $(0, 0, 5)$ / Target: $(0, 0, 0)$
- Post-Process:
    * Bloom: Intensity $0.5 \to 1.5$
    * DepthOfField: Focus distance $3.0$, Bokeh scale $5.0$
- Background Shader: uNoiseScale: $2.0$, uColorA: #050505, uColorB: Brand Color
- Easing: easeInOutSine


3. For Implementation: Integrated Scroll Management Logic (Assuming R3F)
JavaScript
// Control image within useFrame
const progress = scroll.range(0, 1); // Scroll progress of Lenis etc.


// Camera Position Lerp
state.camera.position.lerp(v1.set(
  progress < 0.2 ? 10 : 5, // Threshold per section
  0,
  progress < 0.2 ? 10 : 5
), 0.1);


// Shader Uniforms Update
meshRef.current.material.uniforms.uTime.value = state.clock.getElapsedTime();
meshRef.current.material.uniforms.uProgress.value = progress;
