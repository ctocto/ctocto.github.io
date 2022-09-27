---
title: Three.js 动画
date: 2022-03-15 18:30:00
tags: [Web3D,Three.js,动画]
categories: 饭碗(技术)
---

### Animations


| api | 含义 |
| --- | --- |
| `AnimationMixer` | 作为特定对象的动画混合器，可以管理该对象的所有动画 |
| `AnimationAction` | 为播放器指定对应的片段存储一系列行为，用来指定动画快慢，循环类型等 |
| `AnimationClip` | 表示可重用的动画行为片段，用来指定一个动画的动画效果（放大缩小、上下移动等） |
| `KeyframeTrack` | 与时间相关的帧序列，传入时间和值，应用在指定对象的属性上。目前有 `BooleanKeyframeTrack` `VectorKeyframeTrack` 等。 |

```js
const clock = new THREE.Clock();
const scene = new THREE.Scene();
const renderer = new THREE.WebGLRenderer({
  canvas: document.getElementById('canvas'),
  antialias: true,
  alpha: true,
});
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);

const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000,
);
camera.position.set(0, 0, 5);
camera.lookAt(scene.position);

// 创建纹理
const texture = new THREE.TextureLoader().load(img.src);
// 使用纹理创建贴图
const material = new THREE.SpriteMaterial({ map: texture, color: 0x666666 });
// 使用贴图创建贴图对象
const stone = new THREE.Sprite(material);
stone.name = 'stone';
// 为贴图对象创建动画混合器
const mixer = new THREE.AnimationMixer(stone);

const getClip = () => {
  const times = [0, 1]; // 关键帧时间数组，离散的时间点序列
  const values = [0.6, 0.6, 0.6, 1.2, 1.2, 1.2]; // 与时间点对应的值组成的数组
  // 创建位置关键帧对象：0时刻对应位置0, 0, 0   1时刻对应位置1.2, 1.2, 1.2
  const track = new THREE.VectorKeyframeTrack(`${stone.name}.scale`, times, values);
  const duration = 1;
  return new THREE.AnimationClip('stoneClip', duration, [track]);
};
const mixer = new THREE.AnimationMixer(stone);

const action = mixer.clipAction(getClip());
action.timeScale = 1; // 动画播放一个周期的时间
action.loop = THREE.LoopPingPong; // 动画循环类型
action.play(); // 播放

function animate() {
  renderer.render(scene, camera);
  
  if (mixer) {
    // 更新动画
    const delta = clock.getDelta();
    mixer.update(delta);
  }

  requestAnimationFrame(animate);
}
```

> [Three.js帧动画](http://www.yanhuangxueyuan.com/doc/Three.js/KeyframeTrack.html)
