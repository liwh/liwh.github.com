---
layout: post
title: rspec test
category: other
---

对于测试，我想它的作用，我就不说了。良好的测试可以快速的让我们了解到
我们的程序是否写对，以及后面review时，可以检测会不会对以前的程序造成破会等等。
每个语言都有相应的xunit框架，例如，java里就有 junit.Ruby有Test::Unit,这里，
我们就不说xunit了，我们说说rspec. rspec 是一种ruby的测试dsl,比xunit更好读，且容易描述目的。
不过，最近dhh是提倡使用Test::Unit测试的，可以看看 [dhh-offened-by-spec](http://www.rubyinside.com/dhh-offended-by-rspec-debate-4610.html)

<pre>
  test case 依次执行各个test:
    setup
    exercise
    verify
    teardown
</pre>

```ruby
1.rspec写法
describe Order do
  before do
    @order = Order.new
  end

  context "when initialized" do
    # @order.status.should == "New"
    its(:status){should == "New"}
    # @order.amount.should == 0
    its(:amount){should == 0}
  end

end
```

## describe and context

```ruby
describe Order do
  #通常开头用#表示是instace method,用.开头表示class method
  describe "#amount" do
    #通常用context来加入不同的情景
    context "when user is vip" do
      #测方法的不同分支
      it "should .." do
      end

      it "should .." do
      end
    end

    context "when user is vip" do
    end
  end
end
1.or
describe "A Order" do
  #..
end
```

## before and after

其实就是xunit的setup和teardown,其中before(:each)每段it之前执行，而before(:all)是整段describe执行一次,after相对应。

## pending 用来列出打算要写的测试

## let(:name){exp}

这里一般用来取代before(:each)的,由于它是lazy并且是memoized所以能增加执行速度,当然也有非lazy的let!

```ruby
1.接上
context "when user is vip" do
  let(:user){User.new(:is_vip =>true)}
  let(:order){Order.new(:User =>user)}
  it "should ...." do
    order.total = 200
    order.amount.should == 90000
  end
end

```

## should 还有很多方法

```ruby
xxx.should be_true
xxx.should be_false
xxx.should be_nil
1.xxx.class.should == Array
xxx.should be_a_kind_of(Array)
1.be 开发的should
1.xxx.empty?.should === true
xxx.should be_empty
```

## 输入格式

rspec filename.rb
rspec filename.rb -fs 输出specdoc文件
rspec filename.rb -fh 输入html文件

## 有用的rspec工具

autotest(continuoustesting)
watchr
rcov(查测试代码覆盖率)
rspec-rails(factory_girl,shoulda)

## 注意

一个it一种测试目的
一个错误一个问题
测稳定的方法，这样，当我们变更method时，就不需要改大量的测试了,比如，当我们测第三方的方法时，我们可以把他抽象出来，这样，我们当换了其他的方法时，我们就不需要改测试了
一般不测private method

## 参考资料

[rspec](https://github.com/dchelimsky/rspec)
[rspec info](http://rspec.info)
[railscast rspec](http://railscasts.com/episodes/157-rspec-matchers-macros)
<div style="width:425px" id="__ss_7394497"> <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/ihower/rspec-7394497" title="RSpec 讓你愛上寫測試">RSpec 讓你愛上寫測試</a></strong> <object id="__sse7394497" width="425" height="355"> <param name="movie" value="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=rspec-osdc-110326031940-phpapp01&stripped_title=rspec-7394497&userName=ihower" /> <param name="allowFullScreen" value="true"/> <param name="allowScriptAccess" value="always"/> <embed name="__sse7394497" src="http://static.slidesharecdn.com/swf/ssplayer2.swf?doc=rspec-osdc-110326031940-phpapp01&stripped_title=rspec-7394497&userName=ihower" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="355"></embed> </object> <div style="padding:5px 0 12px"> View more <a href="http://www.slideshare.net/">presentations</a> from <a href="http://www.slideshare.net/ihower">Wen-Tien Chang</a> </div> </div>
