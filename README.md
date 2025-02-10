- LLM token压缩是一个热门方向，但是多模态的token压缩才刚刚出现，目前还没有找到专门的综述文章，所以想在此简单总结一下

- 该仓库主要用来总结VLM上下文压缩领域的文章，梳理清楚业内的多种研究方向和最新进展



对于VLM token的压缩可以按照压缩的阶段进行一个划分

- vision encoder stage
- prefilling stage
- decoding stage

vision encoder stage做的压缩一般是通过merge q-former等方式进行，而prefilling阶段的压缩可解释性明显更强，比如FastV及其之后的很多文章都是通过attention score的方式进行token修剪，而attention score本身就是预训练后的大模型自身的一种先验能力，所以可以认为是利用大模型先验能力进行的压缩。decoding stage压缩的文章我还没怎么看过，最经典的应该就是KV Cache，不过这应该不是token compression领域的文章。

现在的想法是vision encoder stage阶段的压缩粒度可能是比较粗糙的（无论是mlp还是q-former都是利用外部可学习参数进行压缩，当然，还是要看实验效果说话），如果能把LLM本身的先验能力迁移过来进行压缩，可能效果会更好

https://blog.csdn.net/AIBigModel/article/details/144150131





多模态压缩出现的缘由：（参考Multi-Stage Vision Token Dropping: Towards Efficient Multimodal Large Language Model中related work部分内容）

- 随着VLM发展，Gemini（Gemini: A Family of Highly Capable Multimodal Models）和LWM（World model on million-length video and language with ringattention）强调了长上下文对于理解世界的重要性
- 视觉token存在冗余（FastV和 Not all patches are what you need: Expediting vision transformers via token reorganizations）



这引出了一个问题，怎么处理高分辨率的图像输入。（当然，如果我们压缩的是编码后的token，怎么处理其实并不需要关心，但是如果要在编码过程中压缩，比如HiRED，就需要了解一下高分辨率图像的处理逻辑了）

HiRED: Attention-Guided Token Dropping for Efficient Inference of High-Resolution Vision-Language Models in Resource-Constrained Environments一文中总结为dynamic partitioning to encode high-resolution images （比如LLaVA-Next）

![image-20250211002549352](https://s2.loli.net/2025/02/11/M7DTp6JRwoKxfXY.png)