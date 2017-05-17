<div class="page__inner-wrap">
<h1 class="page__title" itemprop="headline">Best Practice - 组件构建
</h1>
<h2 id="组件样例">组件样例</h2>
<p>显示一个带组件位置的文本组件</p>

<h3 id="数据结构">数据结构</h3>

<div class="highlighter-rouge"><pre class="highlight"><code><span class="w">
</span><span class="p">{</span><span class="w">
    </span><span class="nt">"type"</span><span class="p">:</span><span class="s2">"1"</span><span class="p">,</span><span class="w">
    </span><span class="err">//内部的Url</span><span class="w">
    </span><span class="nt">"contentUrl"</span><span class="p">:</span><span class="s2">"https://www.google.com.hk"</span><span class="p">,</span><span class="w">
    </span><span class="err">//跳转地址</span><span class="w">
    </span><span class="nt">"action"</span><span class="p">:</span><span class="s2">"https://iope.tmall.com/campaign-3734-0.htm"</span><span class="w">
</span><span class="p">}</span><span class="w">

</span></code></pre>
</div>

<p>##代码</p>

<h3 id="采用通用-model开发自定义-view">采用通用 model，开发自定义 View</h3>

<div class="highlighter-rouge"><pre class="highlight"><code>//注册组件
builder.registerCell(1, TestView.class);

//组件实现
public class TestView extends FrameLayout implements ITangramViewLifeCycle {
    private TextView textView;
    private BaseCell cell;

    public TestView(Context context) {
        super(context);
        init();
    }

    public TestView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public TestView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init(){
        inflate(getContext(), R.layout.item, this);
        textView = (TextView) findViewById(R.id.title);
    }

    @Override
    public void cellInited(BaseCell cell) {
        setOnClickListener(cell);
        this.cell = cell;
    }

    @Override
    public void postBindView(BaseCell cell) {
        int pos = cell.pos;
        textView.setText(
                cell.id + " pos: " + pos + " " + cell.parent.getClass().getSimpleName() + " " + cell
                        .optParam("msg"));

        if (pos &gt; 57) {
            textView.setBackgroundColor(0x66cccf00 + (pos - 50) * 128);
        } else if (pos % 2 == 0) {
            textView.setBackgroundColor(0xaaaaff55);
        } else {
            textView.setBackgroundColor(0xcceeeeee);
        }
    }

    @Override
    public void postUnBindView(BaseCell cell) {
    }
}
</code></pre>
</div>

<h3 id="采用自定义-model-和自定义-view">采用自定义 model 和自定义 View</h3>

<div class="highlighter-rouge"><pre class="highlight"><code>//注册组件
builder.registerCell(1,
                TestViewHolderCell.class, TextView.class));
                
//组件实现
public class TestViewHolderCell extends BaseCell&lt;TextView&gt; {

    @Override
    public void bindView(@NonNull TextView view) {
        TextView textView = view;
        textView.setText(
                id + " pos: " + pos + " " + parent.getClass().getSimpleName() + " " + optParam(
                        "msg"));

        if (pos &gt; 57) {
            textView.setBackgroundColor(0x66cccf00 + (pos - 50) * 128);
        } else if (pos % 2 == 0) {
            textView.setBackgroundColor(0xaaaaff55);
        } else {
            textView.setBackgroundColor(0xccfafafa);
        }
    }

    @Override
    public void postBindView(@NonNull TextView view) {
        super.postBindView(view);
    }

    @Override
    public void unbindView(@NonNull TextView view) {
        super.unbindView(view);
    }
}
</code></pre>
</div>
