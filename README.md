# tp5_img_handle
tp5 图片处理异常问题解决
  笔者前一阵子做项目的时候遇到一个图片上传并处理问题：用户上传到图片到后台，后台生成缩略图尺寸限制在1000*1000以内。测试的时候发现，因为图片超过2.7M的时候什么错误都不报告，即使打印上传文件参数也没有显示。
  step1:排除上传大小限制，我已经设置的最大图片为 10M以内，$validate=['ext'=>'jpg,jpeg,bmp,png,gif','size'=>'10485760'] 。按理说2.7M远小于限制数值。再者如果因为大小问题报告的错误也会被打印出来。
  step2:在文件上传方法之前打印测试语句 dump(123); 这样的语仍然不显示。删除图片处理语句，可以正常打印出测试语句。所以基本可以判断不是代码问题，应该是服务器问题。
  step3:查找图像处理源码 /vendor/topthink/think-image/src/Image.php,找到82行：$this->im = @$fun($file->getPathname());
  去掉@让系统显示报错，系统给出“PHP Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 8192 bytes)”
  因为上传图片太大，系统在生成缩略图的时候占用内存过多，超出最大范围，系统报告错误。
  
  
  解决办法：
  修改php.ini 文件 找到  memory_limit=32M 改成 你需要的大小或者-1（不限制）
  
