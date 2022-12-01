# 概览
- MeshFilter / MeshRenderer / Material / Mesh相关概念以及基础知识.
- 通过脚本构建一个简单的立方体Mesh.
# Conponents
## MeshFilter(网格过滤器)
从众多网格资源中挑选所需要的Mesh,将Mesh传递给[[Chapter01 被忽视的小伙伴#MeshRenderer|MeshRenderer]],起到了**筛选**的作用.
举例来讲相当于筛选食材的阶段,并将食材交给厨师 (MeshRenderer).
![MeshFilter](https://learning-cdn-public-prd.unitychina.cn/20220816/learn/images/c186a158-4b24-4dfb-92a4-d96fe33fe48f_image.png.2000x0x1.webp)
## MeshRenderer
负责把[[Chapter01 被忽视的小伙伴#MeshFilter(网格过滤器)|MeshFilter]]传递过来的Mesh**绘制**到场景中,举例来说MeshRenderer的作用相当于经验老道的大厨.
![MeshRenderer](https://learning-cdn-public-prd.unitychina.cn/20220816/learn/images/4f499475-4930-4a23-987e-9684afee2637_image.png.2000x0x1.webp)
## Material
Material是[[Chapter01 被忽视的小伙伴#MeshRenderer|MeshRenderer]]中重要的成员,MeshRender的*灵魂*,他决定了Mesh的表面外观是以怎样的质地呈现在玩家的面前,如果材质丢失会出现令无数技术美术落泪的**洋红色(RGB101)**
## Shader(着色器)
Shader是[[Chapter01 被忽视的小伙伴#Material|Material]]的灵魂,既然是Material的灵魂,只要Shader哪里不开心了,即使Material没有丢失Mesh依然会呈现**洋红色**
## MeshData(网格数据)
### Vertices 顶点数组 Vector3[]
存储Mesh的*所有顶点*相关信息
### Topology 拓扑类型
存储的是类型信息,可以理解为存储的是Mesh表面排列组成的结构,构成Mesh的==基本形状==,Unity提供了5种拓扑类型
- **三角面**
- **四边面**
- **线条**
- **虚线**
- **点阵**
### Indices 索引数组 int[]
每个三角面的顶点的索引,可以理解为他存储了构成网格三角面所用到的顶点索引.
### Vertex data 顶点数据
它包含了顶点的位置、法线、切线、UV等属性.
![Data](https://learning-cdn-public-prd.unitychina.cn/20220816/learn/images/ce825e71-1463-4a63-a7a1-a73db85f8de8_image.png.2000x0x1.webp)
### Normal 法线 Vector3[]
垂直于三角面的一台三位向量,他只拥有方向,不包含大小,法线的方向是三角面朝外的方向.
**顶点法线**
垂直于顶点所在的面,方向与该面的法线方向相同
### Tangent 切线 Vector3[]
垂直于法线的一条向量,方向由[[Chapter01 被忽视的小伙伴#UV 纹理坐标 Vector2[]|UV坐标]]来决定
### UV 纹理坐标 Vector2[]
U增长的方向是[[Chapter01 被忽视的小伙伴#Tangent 切线 Vector3[]|切线]]的方向,他和二维空间的X,Y较为类似,是二维坐标系统,模型Mesh除了有三维坐标外,还有即使UV二维坐标,U和V分别代表了Mesh顶点在Texture水平和垂直方向上的采样,坐标通常位于(0,0)[左下角]和(1,1)[右上角]之间,他是Texture映射到模型表面的依据,Mesh顶点会依据UV坐标对Texture进行采样.
### Index data 索引数据
这个数据取决于[[Chapter01 被忽视的小伙伴#Topology 拓扑类型|拓扑类型]]，如果是三角面他储存的就是[0,1,2]，四边形储存的就是[0,1,2,3]，这个索引数值对应的就是顶点数组的下标。
## 实战案例
### 定义立方体顶点数组
立方体由6个面组成,每个四边面被分割成两个三角面,每个三角面包含三个顶点数据,按理说每个面应该拥有*6个*顶点数据,这里只包含4个顶点数据是因为三角面有两个顶点数据是**公用**的;
立方体之所以会用到24个顶点数据是因为四边面的==顶点存在于三个平面==,==顶点数据相同但顶点的法线方向是不相同的==,这个顶点在渲染这3个平面的时候则需要索引到==不同的法线向量==。而在Unity中由于顶点==坐标和法线向量是由同一个索引值取得的==,每个顶点我们要存储三份所以需要24个顶点数据.
### 定义三角面索引数组
要注意的是绕序，这里按顺时针构建的原则，将外面的渲染，里面的剔除。此外要注意和上面构建的顶点数据顺序要一致.
![[Pasted image 20221201130530.png]]
### 定义UV坐标数组
同样这里的UV坐标的顺序也要与顶点数据的顺序相对应
![UV](https://learning-cdn-public-prd.unitychina.cn/20220829/learn/images/881f7fa1-8394-4713-9e65-065412f3e9b5_NR39NZQ7AX____AZYW_D7TW.png.2000x0x1.webp)
![UV](https://learning-cdn-public-prd.unitychina.cn/20220829/learn/images/32ae90d0-bd85-421e-9fb2-59b4d65a3013_V3_51_S5QR_U2Q37H_HF3GH.png.2000x0x1.webp)
### 构建Mesh
将脚本挂载到Sphere上,运行游戏,Sphere被替换成Cube