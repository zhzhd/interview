## 一、基础知识

### 1、Object中有哪些方法，作用是什么

#### （1）hashCode

提供此方法支持的原因是，为了支持一些 hash tables，在不修改 equals 方法的前提下，
在一次程序执行期间，多次调用同一个对象的 hashCode 方法，得到的结果始终应该是一个相同的整数。
如果两个对象的 equals 方法返回值一样，则调用它们各自的 hashCode 方法，返回值也必须保证一样。Object 对象的 hashCode 方法，不同的对象在调用的时候，确实返回了不同的结果，得益于本地实现，返回了对象的内存地址 。
#### （2）equals

equals方法具有以下特性：a：自反性，对于非 null 引用 x， x.equals(x) 应该返回 true。b：对称性，对于非 null 引用 x，y， x.equals(y) 的返回值应该和 y.equals(x) 的返回值保持一致；
#### （3）clone

#### （4）toString

#### （5）notify、notifyAll

#### （6）wait()、wait(long timeout, int nanos)、wait(long timeout)

## 二、多线程

## 三、集合

## 四、IO
