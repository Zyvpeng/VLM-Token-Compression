- LLM token压缩是一个热门方向，但是多模态的token压缩才刚刚出现，目前还没有找到专门的综述文章，所以想在此简单总结一下

- 该仓库主要用来总结VLM上下文压缩领域的文章，梳理清楚业内的多种研究方向和最新进展



对于VLM token的压缩可以按照压缩的阶段进行一个划分

- vision encoder stage
- prefilling stage
- decoding stage

vision encoder stage做的压缩一般是通过merge q-former等方式进行，而prefilling阶段的压缩可解释性明显更强，比例FastV及其之后的很多文章都是通过attention score的方式进行token修剪，而attention score本身就是预训练后的大模型自身的一种先验能力，所以可以认为是利用大模型先验能力进行的压缩。decoding stage压缩的文章我还没怎么看过，最经典的应该就是KV Cache，不过这应该不是token compression领域的文章。

现在的想法是vision encoder stage阶段的压缩粒度可能是比较粗糙的（无论是mlp还是q-former都是利用外部可学习参数进行压缩，当然，还是要看实验效果说话），如果能把LLM本身的先验能力迁移过来进行压缩，可能效果会更好