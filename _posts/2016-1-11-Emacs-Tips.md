 在使用orgmode的时候， latex的公式预览直接使用了系统的latex命令，
 没有使用 /org-latax-pdf-process/ 变量中指定的latex输出命令。
 
 当使用了minted，xcolor包时，需要
{% highlight bash linenos %}
-shell-escape 
{% endhighlight %}
选项。


{% highlight elisp linenos %}
(add-to-list 'org-latex-packages-alist '("" "xcolor" nil))
(add-to-list 'org-latex-packages-alist '("" "minted" nil))
(add-to-list 'org-latex-packages-alist '("" "epstopdf"))
(setq exec-path (append exec-path '("/usr/texbin")))
{% endhighlight %}