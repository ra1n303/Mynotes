格式

cat >> filename << EOF  

![image-20250310082038570](./assets/image-20250310082038570.png)

![image-20250310082041997](./assets/image-20250310082041997.png)

![image-20250310082043912](./assets/image-20250310082043912.png)

后面三个字符EOF随意，但是推荐使用EOF

输入完毕后键入EOF（自己定义的三个字符，推荐使用EOF）退出

![image-20250310082050691](./assets/image-20250310082050691.png)

这种方法支持tab补全





还有一种方法

cat >> filename

![image-20250310082100662](./assets/image-20250310082100662.png)

这种方法不支持tab补全

同时也不是EOF结束，而是在输入完毕后的新一行ctrl+d