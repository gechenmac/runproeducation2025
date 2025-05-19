# Vue Ant Design Vue 应用

这是一个基于 Vue 3 和 Ant Design Vue 构建的应用程序。

## GSAP 动画系统

本项目使用 GSAP (GreenSock Animation Platform) 实现了一个可复用的动画系统，让各组件轻松实现复杂动画效果。

### 动画系统架构

动画系统由以下部分组成：

1. **gsap.js** - 基础动画函数库
   - 包含常用的动画效果（淡入、从不同方向进入等）
   - 提供标准化的动画配置选项

2. **useScrollTrigger.js** - 滚动触发器
   - 提供基于滚动位置触发动画的功能
   - 管理动画的创建和清理

3. **useAnimation.js** - 全局动画组合式API
   - 将基础动画和滚动触发功能整合为高级API
   - 提供简洁的接口供组件使用

### 使用方法

#### 基本用法

在组件中导入并使用动画系统：

```vue
<script setup>
import { ref, onMounted } from 'vue';
import { useAnimation } from '../composables/useAnimation';

// 元素引用
const elementRef = ref(null);

// 获取动画工具
const { animate } = useAnimation();

onMounted(() => {
  // 为元素添加淡入动画
  animate(elementRef.value, {
    direction: 'up', // 从下方淡入
    duration: 1,
    delay: 0.2
  });
});
</script>
```

#### 滚动触发动画

```vue
<script setup>
import { ref, onMounted } from 'vue';
import { useAnimation } from '../composables/useAnimation';

const elementRef = ref(null);
const { animateOnScroll } = useAnimation();

onMounted(() => {
  // 滚动到元素位置时触发动画
  animateOnScroll(elementRef.value, {
    direction: 'left', // 从左侧淡入
    scrollOptions: {
      start: 'top 75%' // 当元素顶部到达视窗75%位置时
    }
  });
});
</script>
```

#### About 组件风格动画

```vue
<script setup>
import { ref, onMounted } from 'vue';
import { useAnimation } from '../composables/useAnimation';

const containerRef = ref(null);
const leftRef = ref(null);
const rightRef = ref(null);

const { animateAboutSection } = useAnimation();

onMounted(() => {
  // 应用左右元素的交错入场动画
  animateAboutSection(
    containerRef.value,
    leftRef.value,
    rightRef.value
  );
});
</script>
```

#### 交错元素动画

```vue
<script setup>
import { ref, onMounted } from 'vue';
import { useAnimation } from '../composables/useAnimation';

const containerRef = ref(null);
const itemRefs = ref([]);

const { animateStaggeredOnScroll } = useAnimation();

onMounted(() => {
  // 滚动触发元素逐个入场
  animateStaggeredOnScroll(
    containerRef.value,
    itemRefs.value,
    {
      stagger: 0.15, // 每个元素的延迟时间
      duration: 0.8
    }
  );
});
</script>
```

### 动画配置选项

所有动画函数都支持以下常见配置：

- `duration` - 动画持续时间（秒）
- `delay` - 动画开始前的延迟时间（秒）
- `ease` - 缓动函数，如 'power2.out'、'back.out' 等
- `direction` - 动画方向 ('up', 'down', 'left', 'right', 'scale', 'fade')

滚动触发配置：

- `scrollOptions.start` - 触发动画的滚动位置，如 'top 75%'
- `scrollOptions.once` - 是否只触发一次
- `scrollOptions.markers` - 是否显示调试标记（开发时有用）

### 示例

查看示例组件了解各种动画的使用方法：

- `About.vue` - 左右内容的入场动画
- `SampleSection.vue` - 综合示例，展示不同类型的动画 