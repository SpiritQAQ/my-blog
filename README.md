# my-blog
多终端的同步只需要对分支hexo进行操作
此时在另一终端更新博客，只需要将Github的hexo分支clone下来，进行初次的相关配置
```
git clone 
npm install 
git add source  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
git push origin hexo  //更新分支
hexo d -g
```
