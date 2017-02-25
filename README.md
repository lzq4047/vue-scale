# vue-scale

> a scale,drag component for vue based on css3

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 用法

### 基本用法

```html
<scale>
  <div>
    content
  </div>
</scale>
```

### 设置长按缩放的响应时间

```html
<scale :quick-scale-timeout="1500">
  <div>
    content
  </div>
</scale>
```

### 设置长按缩放的速度
```
<scale :quick-scale-interval="30">
  <div>
    content
  </div>
</scale>
```

## 属性

| 名称 | 描述 | 类型 | 可选值 | 默认值 |
|------|------------|------|----------------|---------|
|step  | 缩放的递增和递减每次的改变量 | Number | > 0 | 0.1 |
|quick-scale-timeout | 长按响应时间 | Number | > 0 | 1000 |
|quick-scale-interval | 长按是缩放速度 | Number | > 0 | 100 |
