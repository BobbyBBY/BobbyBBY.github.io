---
layout: page
title: About me
# subtitle: Hi
---

<div class="about-content">
  <p class="about-paragraph drop-cap">这个博客记录着我认为重要或者有趣的知识，每篇文章都融入了我的理解。在攥写文章时我尽量保持内容通俗易懂，因为我希望我的读者来自各行各业，可以对不同方面的知识提出建议。</p>
  <p class="about-paragraph">由于博客不限主题，或许看起来有些繁杂，但每篇博客都不是单纯转载，我相信一定有你喜欢的一部分。</p>
</div>

<div class="share-card">
  <p class="share-card-text"><i class="fas fa-paper-plane"></i> 如果你觉得本站内容有价值，欢迎复制链接推荐给朋友：</p>
  <div class="share-card-url-container">
    <span class="share-card-url">bobbybby.top</span>
    <button class="share-card-btn" id="copy-btn"><i class="far fa-copy"></i> 复制本站链接</button>
  </div>
</div>

<div class="toast-tip" id="toast-tip">
  <i class="fas fa-check-circle"></i> 链接复制成功，感谢您的推荐！
</div>

<script>
document.getElementById('copy-btn').addEventListener('click', function() {
  const url = "https://bobbybby.top";
  navigator.clipboard.writeText(url).then(function() {
    const toast = document.getElementById('toast-tip');
    toast.classList.add('show');
    setTimeout(function() {
      toast.classList.remove('show');
    }, 2000);
  }).catch(function(err) {
    console.error('Could not copy text: ', err);
  });
});
</script>
