# js

- [防抖与节流](#防抖与节流)

### 防抖与节流
防抖与节流函数是一种最常用的 高频触发优化方式，能对性能有较大的帮助。

- **防抖 (debounce)**: 将多次高频操作优化为只在最后一次执行，通常使用的场景是：用户输入，只需再输入完成后做一次输入校验即可。

        export const debounce = (delay) => {
            return (target, name, descriptor) => {
                const oldValue = descriptor.value;
                let timer = null;
                descriptor.value = function (...args) {
                    if (timer) {
                        clearTimeout(timer);
                    }
                    clearTimeout(timer);
                    timer = setTimeout(() => {
                        oldValue.apply(this, args);
                    }, delay);
                };
                return descriptor;
            }
        }

- **节流(throttle):** 每隔一段时间后执行一次，也就是降低频率，将高频操作优化成低频操作，通常使用场景: 滚动条事件 或者 resize 事件，通常每隔 100~500 ms执行一次即可。

        export const throttle = (delay) => {
            return (target, name, descriptor) => {
                const oldValue = descriptor.value;
                let timer = null,
                    remaining = 0,
                    previous = +new Date();
                descriptor.value = function (...args) {
                    let now = +new Date(),
                        remaining = now - previous;
                    if (remaining >= delay) {
                        if (timer) {
                            clearTimeout(timer);
                        }
                        oldValue.apply(this, args);
                        previous = now;
                    } else {
                        if (!timer) {
                            timer = setTimeout(() => {
                                oldValue.apply(this, args);
                                previous = +new Date();
                            }, delay - remaining);
                        }
                    }
                };
                return descriptor;
            }
        }

    用法：

        export class AppComponent implements OnInit {

            ngOnInit() {}

            @throttle(500)
            throttleChange(value: any) {
                console.log(value);
            }

            @debounce(500)
            debounceChange(value: any) {
                console.log(value);
            }
        }