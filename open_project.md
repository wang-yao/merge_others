<a href="http://tangram.pingguohe.net/">Tangram from alibaba</a> <br>
![](https://img.alicdn.com/tfs/TB1v8lrQpXXXXcEXFXXXXXXXXXX-600-1067.gif) <br>
[***使用指南*** ](http://tangram.pingguohe.net/docs/android/access-tangram) <br>
---   
<div class="page__inner-wrap">
<h1 class="page__title" itemprop="headline">接入Tangram代码
</h1>
<h2 id="1引入依赖">1.引入依赖</h2>

<div class="highlighter-rouge"><pre class="highlight"><code>// gradle
compile 'com.alibaba.android:tangram:1.0.0@aar'
</code></pre>
</div>

<p>或者</p>

<div class="highlighter-rouge"><pre class="highlight"><code>// maven
&lt;dependency&gt;
  &lt;groupId&gt;com.alibaba.android&lt;/groupId&gt;
  &lt;artifactId&gt;tangram&lt;/artifactId&gt;
  &lt;version&gt;1.0.0&lt;/version&gt;
  &lt;type&gt;aar&lt;/type&gt;
&lt;/dependency&gt;
</code></pre>
</div>

<h2 id="2初始化-tangram-环境">2.初始化 Tangram 环境</h2>

<p>应用全局只需要初始化一次，提供一个通用的图片加载器，一个应用内通用的ImageView类型（通常情况下每个应用都有自定义的 ImageView，如果没有的话就提供系统的 ImageView 类）。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>TangramBuilder.init(context, new IInnerImageSetter() {
	@Override
	public &lt;IMAGE extends ImageView&gt; void doLoadImageUrl(@NonNull IMAGE view,
                    @Nullable String url) {
		//假设你使用 Picasso 加载图片
		Picasso.with(context).load(url).into(view);
	}
}, ImageView.class);
</code></pre>
</div>

<h2 id="3初始化-tangrambuilder">3.初始化 <code class="highlighter-rouge">TangramBuilder</code></h2>

<p>在 Activity 中初始化<code class="highlighter-rouge">TangramBuilder</code>，假设你的 Activity 是<code class="highlighter-rouge">TangramActivity</code>。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>TangramBuilder.InnerBuilder builder = TangramBuilder.newInnerBuilder(TangramActivity.this);
</code></pre>
</div>

<p>这一步 builder 对象生成的时候，内部已经注册了框架所支持的所有组件和卡片，以及默认的<code class="highlighter-rouge">IAdapterBuilder</code>（它被用来创建 绑定到 RecyclerView 的Adapter）。</p>

<h2 id="4注册自定义的卡片和组件">4.注册自定义的卡片和组件</h2>

<p>一般情况下，内置卡片的类型已经满足大部分场景了，业务方主要是注册一下自定义组件。注册组件有3种方式：</p>

<ul>
  <li>注册绑定组件类型和自定义<code class="highlighter-rouge">View</code>，比如<code class="highlighter-rouge">builder.registerCell(1, TestView.class);</code>。意思是类型为1的组件渲染时会被绑定到<code class="highlighter-rouge">TestView</code>的实例上，这种方式注册的组件使用通用的组件模型<code class="highlighter-rouge">BaseCell</code>。</li>
  <li>注册绑定组件类型、自定义 model、自定义<code class="highlighter-rouge">View</code>，比如<code class="highlighter-rouge">builder.registerCell(1, TestCell.class, TestView.class);</code>。意思是类型为1的组件使用自定义的组件模型<code class="highlighter-rouge">TestCell</code>，它应当继承于<code class="highlighter-rouge">BaseCell</code>，在渲染时会被绑定到<code class="highlighter-rouge">TestView</code>的实例上。</li>
  <li>注册绑定组件类型、自定义model、自定义<code class="highlighter-rouge">ViewHolder</code>，比如<code class="highlighter-rouge">builder.registerCell(1, TestCell.class, new ViewHolderCreator&lt;&gt;(R.layout.item_holder, TestViewHolder.class, TestView.class));</code>。意思是类型为1的组件使用自定义的组件模型<code class="highlighter-rouge">TestCell</code>，它应当继承于<code class="highlighter-rouge">BaseCell</code>，在渲染时以<code class="highlighter-rouge">R.layout.item_holder</code>为布局创建类型为<code class="highlighter-rouge">TestView</code>的 view，并绑定到类型为<code class="highlighter-rouge">TestViewHolder</code>的 viewHolder 上，组件数据被绑定到定到<code class="highlighter-rouge">TestView</code>的实例上。</li>
</ul>

<p>一般情况下，使用前两种方式注册组件即可。至于组件开发规范，请参考<a href="/docs/android/develop-component">组件开发</a>。</p>

<h2 id="5生成tangramengine实例">5.生成<code class="highlighter-rouge">TangramEngine</code>实例</h2>

<p>在上述基础上调用：</p>

<div class="highlighter-rouge"><pre class="highlight"><code>TangramEngine engine = builder.build();
</code></pre>
</div>

<h2 id="6绑定业务-support-类到-engine">6.绑定业务 support 类到 engine</h2>

<p>Tangram 内部提供了一些常用的 support 类辅助业务开发，业务方也可以自定义所需要的功能模块注册进去。以下常用三个常用的support，分别处理点击、卡片数据加载、曝光逻辑，详情请参考<a href="">文档</a>。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>engine.register(SimpleClickSupport.class, new XXClickSupport());
engine.register(CardLoadSupport.class, new XXCardLoadSupport());
engine.register(ExposureSupport.class, new XXExposureSuport());
</code></pre>
</div>

<h2 id="7绑定-recyclerview">7.绑定 recyclerView</h2>

<div class="highlighter-rouge"><pre class="highlight"><code>setContentView(R.layout.main_activity);
RecyclerView recyclerView = (RecyclerView) findViewById(R.id.main_view);
...
engine.bindView(recyclerView);
</code></pre>
</div>

<h2 id="8监听-recyclerview-的滚动事件">8.监听 recyclerView 的滚动事件</h2>

<div class="highlighter-rouge"><pre class="highlight"><code>recyclerView.addOnScrollListener(new RecyclerView.OnScrollListener() {
	@Override
	public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
		super.onScrolled(recyclerView, dx, dy);
		//在 scroll 事件中触发 engine 的 onScroll，内部会触发需要异步加载的卡片去提前加载数据
		engine.onScrolled();
	}
});
</code></pre>
</div>

<h2 id="9设置悬浮类型布局的偏移可选">9.设置悬浮类型布局的偏移（可选）</h2>

<p>如果你的 recyclerView 上方还覆盖有其他 view，比如底部的 tabbar 或者顶部的 actionbar，为了防止悬浮类 view 和这些外部 view 重叠，可以设置一个偏移量。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>engine.getLayoutManager().setFixOffset(0, 40, 0, 0);
</code></pre>
</div>

<h2 id="10设置卡片预加载的偏移量可选">10.设置卡片预加载的偏移量（可选）</h2>

<p>在页面滚动过程中触发<code class="highlighter-rouge">engine.onScrolled()</code>方法，会去寻找屏幕外需要异步加载数据的卡片，默认往下寻找5个，让数据预加载出来，可以修改这个偏移量。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>engine.setPreLoadNumber(3)
</code></pre>
</div>

<h1 id="11加载数据并传递给-engine">11.加载数据并传递给 engine</h1>

<p>数据一般是调用接口加载远程数据，这里演示的是 mock 加载本地的数据：</p>

<div class="highlighter-rouge"><pre class="highlight"><code>String json = new String(getAssertsFile(this, "data.json"));
        JSONArray data = null;
        try {
            data = new JSONArray(json);
            engine.setData(data);
        } catch (JSONException e) {
            e.printStackTrace();
        }
</code></pre>
</div>
---  

<div class="page__inner-wrap">
 
 <h1 class="page__title" itemprop="headline">TangramView核心方法
</h1>
      
 <h2 id="tangrambuilder-核心方法">TangramBuilder 核心方法</h2>

<p><code class="highlighter-rouge">TangramBuilder</code>是初始化 Tangram 运行环境，初始化<code class="highlighter-rouge">TangramEngine</code>的模块，它有一些核心方法，必须重点介绍。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public static void init(@NonNull final Context context, IInnerImageSetter innerImageSetter, Class&lt;? extends ImageView&gt; imageClazz)
</code></pre>
</div>

<p>初始化 Tangram，初始化在应用全局只需要调用一次即可。主要提供一个<a href=""><code class="highlighter-rouge">InnerImageSetter</code></a>，它是加载图片的通用接口，用来在框架内部加载图片时调用。还要提供一个图片基类，通常一个应用会自定义一个 ImageView，为了让 Tangram 内部也能使用业务方自定义的 <code class="highlighter-rouge">ImageView</code>，在初始化的时侯也将该类传给 Tangram。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public static InnerBuilder newInnerBuilder(@NonNull final Context context)
</code></pre>
</div>

<p>构造真正的 builder 对象，返回的<code class="highlighter-rouge">InnerBuilder</code>对象里已经注册了 Tangram 内置的卡片，业务方拿到这个 buidler 对象主要是去注册自定义的卡片和组件。最后调用<code class="highlighter-rouge">public TangramEngine build()</code>生成<code class="highlighter-rouge">TangramEngine</code>。</p>

<hr />

<h2 id="tangramengine-核心方法">TangramEngine 核心方法</h2>

<p><code class="highlighter-rouge">TangramEngine</code>是绑定 Tangram 数据，绑定 RecyclerView 的核心模块，也是操作页面数据的核心模块，它的一些核心方法，也必须重点介绍。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public void bindView(@NonNull final RecyclerView view)
</code></pre>
</div>

<p>绑定 recyclerView 对象。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public void unbindView()
</code></pre>
</div>

<p>清空所绑定的 recyclerView 对象、adapter对象等。</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public RecyclerView getContentView()
</code></pre>
</div>

<p>获取所绑定的 recyclerView 对象，这一步应当在调用 bindView 之后调用。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public void destroy()
</code></pre>
</div>

<p>页面退出时调用，释放内部资源。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public List&lt;C&gt; parseData(@Nullable T data)
</code></pre>
</div>
<p>解析卡片json数据</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public List&lt;L&gt; parseComponent(@Nullable T data)
</code></pre>
</div>
<p>解析组件json数据</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public void setData(@Nullable T data)
</code></pre>
</div>
<p>绑定整个页面的数据</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public void appendData(@Nullable T data)
</code></pre>
</div>
<p>添加卡片数据</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public void insertData(int position, @Nullable T data)
</code></pre>
</div>
<p>插入卡片数据</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public void replaceData(int position, @Nullable T data)
</code></pre>
</div>
<p>替换卡片数据</p>

<p>上述几个方法都有原始json 数据类型和解析过的卡片 model 数据类型的版本。</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public &lt;S&gt; void register(@NonNull Class&lt;S&gt; type, @NonNull S service)
</code></pre>
</div>

<p>注册自定义的辅助模块</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public &lt;S&gt; S getService(@NonNull Class&lt;S&gt; type)
</code></pre>
</div>
<p>获取注册的辅助模块</p>

<hr />

<div class="highlighter-rouge"><pre class="highlight"><code>public void refresh()
</code></pre>
</div>
<p>刷新页面</p>

<div class="highlighter-rouge"><pre class="highlight"><code>public void onScrolled()
</code></pre>
</div>
<p>触发页面懒加载数据的逻辑</p>
