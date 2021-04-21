---
title: Contact From 7 如何在表单中添加 referer page 信息?
author: Oneuie
date: 2021-4-21 11:22:00 +0800
categories: [Wordpress,插件]
tags: [Wordpress,Contactfrom7]
---

平时我们使用contact from 7可以得到一封非常完善的询盘信息。但是如何了解访客是`通过哪一个页面`进入我们的`发送询盘的页面`的呢？

**这里就需要我们来操作一下达到我们的的需求了。**

## 添加代码到`function.php`

将下方的代码添加到主题的`function.php`文件中,

```php
// referer page contact from 7
function getRefererPage( $form_tag )
{
if ( $form_tag['name'] == 'referer-page' ) {
$form_tag['values'][] = htmlspecialchars($_SERVER['HTTP_REFERER']);
}
return $form_tag;
}
if ( !is_admin() ) {
add_filter( 'wpcf7_form_tag', 'getRefererPage' );
}
```
## 添加代码到`表单`

完成上方的步骤之后，将下方的代码添加在所需要的表单中，并且保存.

```
[hidden referer-page default:get]
```

![Contactfrom7](/assets/img/post/contact%20from%207.jpg)

## 编辑表单的邮件模版

在适当的位置添加 `[referer-page]` 并且保存。

现在通过给这个表单给你发的邮件，就可以看到referer-page. 

## 表单参考模版

- **表单参考**

```markdown
<p class="cf7-title"><h2 style="font-size:25px;"><span style="color: #ffffff;"><i class="fa fa-envelope-o"></i> We would love to work with you.</span></h2></p>
<p>[text* your-name placeholder "Your name*"]</p>
<p>[email* your-email placeholder "Email Address*"]</p>
<p>[tel tel-551 placeholder "Telephone"]</p>
<p>[text your-subject placeholder "Title"]</p>
[hidden referer-page default:get]
<p>[textarea* your-message placeholder "Wirtring your requrie"]</p>
<p>[submit "Send"]</p>
```
- **邮件模版参考**

```markdown
Hello, My firend,

My name is [your-name]. I am interested in [your-subject]. Following is my detail require:
[your-message].

And please found [your-name]'s contact information as following:
Name:[your-name]
Phone Number: [tel-551]
Email Address: [your-email]

After I visit [referer-page]. I write message from [_url] and the title is [_post_title].

Thank you for your reply.

Best Regards,

[your-name]

My IP: [_remote_ip]
Browser information: [_user_agent]
-- 
This e-mail was sent from a contact form on Oneuie.me (https://oneuie.me)
```