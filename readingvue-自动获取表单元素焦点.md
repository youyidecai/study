移动端项目中需要满足弹出框中的textarea和input自动获取焦点，键盘自动弹出。

 直接用如下代码：
 
     new Vue({
          el: '#vueApp',
          data: {
              showingDialogBox: false,
          },
          methods: {
              showDialogBox: function() {
                  this.showingDialogBox = true;
                  // auto focus
                  var object = this.$els.nameInput;
                  object.focus();
              }
          }
      });
 
没有起到自动获取焦点的作用。原因是DOM尚未被更新，input还是隐藏状态呢。

可以用`nextTick`，这样回调函数在 DOM 更新完成后就会调用。下面是实现的代码：
 
      new Vue({
          el: '#vueApp',
          data: {
              showingDialogBox: false,
          },
          methods: {
              showDialogBox: function() {
                  this.showingDialogBox = true;
                  // auto focus
                  var object = this.$els.nameInput;
                  Vue.nextTick(function() {
                      object.focus();
                  });
              }
          }
      });
 
 这样，就可以自动获取到焦点，弹出键盘了。
 
 
