<div>
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
