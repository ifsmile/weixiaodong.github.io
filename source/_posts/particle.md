---
title: particle
date: 2018-02-25 10:27:34
tags:
    - JavaScript
cover_picture: images/particle.png
summary: 此文章主要讲解如何用js实现粒子背景的功能。代码比较简单，自己可以根据注释调节粒子的大小、移动速度、粒子连线的最大距离等效果。此代码是本人一时兴起写的，各位帅哥美女如果有好的见解请与本人联系，共同商讨改进，谢谢！
---
此文章主要讲解如何用js实现粒子背景的功能。
代码比较简单，自己可以根据注释调节粒子的大小、移动速度、粒子连线的最大距离等效果。
此代码是本人一时兴起写的，各位帅哥美女如果有好的见解请与本人联系，共同商讨改进，谢谢！

## 代码

```html
<body style="background:#333;">
    <canvas width="1000" height="800" id="canvas"></canvas>
    <script>
        const canvas = document.querySelector('#canvas')
        const ctx = canvas.getContext('2d')
        const w = canvas.offsetWidth
        const h = canvas.offsetHeight
        canvas.width = w
        canvas.height = h
        
        class Point {
            constructor(x, y) {
                this.x = x // 点的坐标
                this.y = y
                this.r = Math.random() * 2 + 1  // 点的半径
                this.sx = Math.random() * 4 -2 // 移动速度
                this.sy = Math.random() * 4 -2
            }
            draw() { // 画点
                ctx.beginPath()
                ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2)
                ctx.closePath()
                ctx.fillStyle = '#f00'
                ctx.fill()
            }
            move() { // 点移动
                this.x += this.sx
                this.y += this.sy
                if(this.x < 0 || this.x > w) this.sx = -this.sx
                if(this.y < 0 || this.y > h) this.sy = -this.sy
            }
            drawLine(one, another) { // 划线
                // 计算两点间距离
                const d = Math.sqrt((one.x - another.x) * (one.x - another.x) + (one.y - another.y) * (one.y - another.y))
                // 两点间距离小于300时划线
                if(d < 300 && one !==  another) {
                    ctx.beginPath()
                    ctx.moveTo(one.x, one.y)
                    ctx.lineTo(another.x, another.y)
                    ctx.closePath()
                    ctx.strokeStyle = `rgba(226, 72, 72, ${Math.random() / 3})`
                    ctx.strokeWidth = 1
                    ctx.stroke()
                }
            }
        }

        let points = []

        for(let i = 40; i > 0; i --) {
            points.push(new Point(Math.random() * w, Math.random() * h))
        }
        function paint() {
            ctx.clearRect(0, 0, w, h)
            for(let i in points) {
                points[i].move()
                points[i].draw()
                for(let j in points) {
                    points[i].drawLine(points[i], points[j])
                }
            }
            // 下次重绘签再次调用此函数
            requestAnimationFrame(paint)
        }
        paint()
    </script>
</body>
```