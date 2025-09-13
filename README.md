# awesome-ai-safety
Awesome AI Safety List, extracted from top conferences and journals

- Jailbreak的一些想法
    - 到底什么是有害，万一用户真的意图是好的呢？那么直接拒绝回答是否合适，还是在遇到举棋不定的prompt时通过告知并征询的方式呢？
    - 如何提高defense后提高回答的质量，做个benchmark？
    - 通过machine unlearning的方式来defense吗
    - 如何提高性能在defense的情况下
    - Judge LLM准不准
    - harmful的东西是否无法避免
    - attack有很多，一个defense能defense所有吗？不能的话会不会代价很大呢？
    - 如果让一个模型把所有defense strategy全部加上，比如对不同attack类型都进行fine tuning。会影响多少utility？
    - 不要相信论文，如果论文说不可行，那就自己做一遍，如果可行，那就可以发论文了
    - bias的benchmark
    - 如何defend multi shot
    - multi shot的style format对attack对影响
    - reasoning model，multimodal和agent对jailbreak
    - RAG如果有害怎么办
    - 研究defense的transferability?
    - 研究judge llm
    - 如果没发现prompt有害，如何判断output有害？是否用一个reasoning model来判断output有害。这个model对所有attack都熟悉，而且一有新的attack就可以加上。在用这个reasoning model来训练model让model更robust
    - reasoning model有自我进化的能力，自己做synthetic data，然后自己reinforcement learning
    - legal和safety似乎是两个方面，是否可以做一些legal相关的research
    - 是否可以研究生成logits的distribution，在safe和unsafe上
    - 可以研究activation

## Survey

- A Survey on Model Extraction Attacks and Defenses for Large Language Models
    
    ![Screenshot 2025-09-04 at 15.56.27.png](attachment:b84fe6af-0376-432f-b916-8f40340f8997:Screenshot_2025-09-04_at_15.56.27.png)
    
    - 全文讲述了如何偷模型和防止被偷，感觉偷起来还是很容易的，因为大家都有transformer架构，只要把Query和Answer的数据偷到手，再通过和模型App的interaction，就可以吧prompt偷到，最后用更少的钱distill训练模型
    - AI agent能不能被偷呢？
    - 很难steal weights or architecture
    - attention mechanism 不可避免会泄漏信息，因为它记住了所有东西
    - 不懂如何model可以自动遗忘敏感信息
- Jailbreak Attacks and Defenses Against Large  Language Models: A Survey
    
    ![Screenshot 2025-09-07 at 11.21.01.png](attachment:ae982c03-93e4-4014-aed0-2985de534033:Screenshot_2025-09-07_at_11.21.01.png)
    
    - 每一个方法都不难理解，attack基本上都是prompt engineering和fine tuning。defense基本上都是prompt检测和fine tuning。
    - Proxy Defense最简单，就加上一个外部的模型来分别检测input和output，并决定通不通过
    - Gradient-based基本上就是加suffix，比较早期。各种Prompt Rewriting，不过是模型自动生成prompt，还是用情景，如密码，coding，少见语言，都是比较简单的
- A Survey on Trustworthy LLM Agents: Threats and Countermeasures
    - 文章从Brain，Memory和Tool讲述了Attack和Defense以及Evaluation。非常通俗易懂。
    - Extrinsic Trustworthy比较水，可以不看
    - multi agent也就是多agent之间交流如何变得安全
    - 未来研究方向
        - 建立动态的benchmark: However, given the agent brain module’s frequently dynamic interactions with external information, such evaluations are clearly insufficient. To address this, dynamic evaluation mechanisms should be implemented to better simulate real-world scenarios. For example, a continuous learning based evaluation framework can be designed, where agents are tested in real-time environments with evolving data streams.
        - Current memory-centric attack methods often lack generalization across tasks. Memory poisoning relies on task-specific malicious samples to ensure retrieval by relevant task queries. Memory misuse, on the other hand, heavily depends on dialogue context, requiring tailored designs for various scenarios.
        - 从数据库防止恶意数据：From the defense perspective, for memory poisoning defense, future defenses should focus on the vector database end to prevent the injection of toxic samples, thereby reducing the defense time during agent responses.
        - For privacy leakage defense, privacy protection mechanisms such as query rewriting or fine-tuning LLMs must be implemented in applications where privacy breaches are possible.
        - For memory misuse defense, developing a multi-round adversarial dialogue training paradigm for safety alignment is essential to enhance robustness of agent.
        - 简历memory的benchmark：For the evaluation, as there is currently no systematic and reliable benchmarks, we recommend establishing benchmarks for the paradigms of attacks and defenses mentioned above.
        - 缺少工具相关的防御：Our investigation reveals that research on defenses against tool-related attacks is notably scarce, whether in the context of LLMs, agents, or MAS. GuardAgent [105] takes the first step
    
    ![Screenshot 2025-09-09 at 22.27.35.png](attachment:a5bf0303-fed3-4b28-901e-490be84cc272:Screenshot_2025-09-09_at_22.27.35.png)
    
- Machine Unlearning in Generative AI: A Survey
    - 感觉需要很多数学，数学公式看不太懂，哈哈哈
    
    ![Screenshot 2025-09-10 at 11.47.10.png](attachment:4cd64035-78c2-4af5-9fcd-8702b1a090ac:Screenshot_2025-09-10_at_11.47.10.png)
    

## ICML 2025

Keyword：**Judge**

- Learning to Plan & Reason for Evaluation with Thinking-LLM-as-a-**Judge**
    - 这篇文章指出Judge LLM如果是reasoning model的话，缺少具体的planning evaluation的能力
    - 这篇文章讲述了如何用reasoning model作为Judge来evaluate 不同的llm模型
    - 用Reasoning Model的self-improving能力，也就是自我进化来让他变成更好的Judge。就是让模型生成多个plan，execution，verdict的sample（有对有错），然后用这些sample来自我训练
    - 所以，所用的训练data都是synthetic的
- Tuning LLM **Judge** Design Decisions for 1/1000 of the Cost
    - Judge LLM的evaluation的能力很难界定，引文要人为去看，而且每一个Judge都得在多个model上拿到evaluations才知这个Judge怎么样
    - 不是很懂这篇文章怎么中的顶会，难道不就是调调hyperparameters吗
    - 选了两个metric：Spearman correlation和human aggreement

keyword：**defense**

- AutoAdvExBench: Benchmarking autonomous exploitation  of adversarial example **defenses**
    - 说实话，和defense没什么关系。这更像是一个agent的benchmark。看llm有没有可能在知道defense方式的情况下jailbreak attack成功
    - 感觉可以做其他defense的benchmark，也用agent？
- Bi-perspective Splitting **Defense**: Achieving Clean-Seed-Free Backdoor Security
    - 这篇文章用了三种非常巧妙方法来吧poisoned data从数据集中筛选出来
    - 一种是发现poisoned data的第二高的选项consistently是original的
    - 第二种是同过cluster把离中心远的数据筛掉
    - 第三种是找出hard clean data也就是loss高的clean data，然后用altruistic model（只trained on poisoned data）排除loss低的
- MELON: Provable **Defense** Against Indirect Prompt Injection Attacks in AI Agents
    - 很巧妙的方法避开了malicious tool给出的malicious repsonse对接下来的tool调用的影响
    - 但感觉不实用，很理想化的。简单来说就是agent别用malicious tool不就行了吗
- Adaptive Median Smoothing: Adversarial **Defense** for  Unlearned Text-to-Image Diffusion Models at Inference Time
    - 很水，就是perturb一些不大好大词比如erotic

Keyword：**jailbreak**

- The **Jailbreak** Tax: How Useful are Your **Jailbreak** Outputs?
    - 这篇文章不难做，但问题提的很好，然后解决方法也很巧妙，把math和bio变成harmful的content，并且用了一些trick如evilmath
    - 很难判断jailbreak后的utility是否会降低，1是因为你要有一个unaligned的model来证明生成出道harmful回答质量不好，2是很难判断什么是harmful
    - 这篇文章通过用unligned的model但是是在math上的。发现jailbreak后model回答math问题并不好，所以证明Jailbreak后的utility会降低
    - 发现不同attack的utility loss降低程度不同
    - 这么文章解决了how useful are jailbreak outputs。那是不是可以写一篇文章how useful are jailbreak defense outputs呢？You Can’t Eat Your Cake and Have It Too: The Performance degradation of Jailbreak Defense
    - Many Shot 几乎没有degradation
    - 是否可以用真实的harmful dataset来做实验呢？
- You Can’t Eat Your Cake and Have It Too: The Performance Degradation of LLMs with **Jailbreak** Defense
    - 用了jailbreak defense后模型输出的质量有显著下滑，computation resources消耗量和safety提高不成正比，会让输出变慢影响user experience
    - 未来方向是加强safety的情况下让utility变高
- Mitigating Many-Shot **Jailbreaking**
    - 面对many-shot jailbreaking，finetuning其实是有效的，input sanitizatio（也就是不让attacker获得真实role tag如<user>,<assistant>）也起一定效果
    - 不是很懂NumAttack和NumShot是什么，感觉建dataset有点复杂
- PANDAS: Improving Many-shot **Jailbreaking** via  Positive Affirmation, Negative Demonstration, and Adaptive Sampling
    - 设计了一个ManyHarm，harmful的数据对
    - 在many shots的对话里设计了一些新的trick，比如positive affirmation（就是对话不断强调对对对），negative demonstration（迂回战术）
- SELFDEFEND: LLMs Can Defend Themselves against **Jailbreaking**  in a Practical Manner
    - 定义defense通过4个不同的objectives，很新颖
    - 这篇文章的方法就是在model generate output的时候**concurrently**用另一个model来检测prompt和output，这样就减少生成的反应时间，也就是不用等filter了。
    - 为了发安全的会，还说这个方法是受shadow attack的inspiration
- Emoji Attack: Enhancing **Jailbreak** Attacks Against Judge LLM Detection
    - 很简单，水文章，就是加点emoji去混淆judge llm
    - 如何defend呢？
    - emoji到底意味什么，为什么有用？
    - 可以具体研究token segmentation bias
    - 一定要emoji吗，其他的可以吗
- Weak-to-Strong **Jailbreaking** on Large Language Models
    - 一个成果就是算了token distribution的KL Divergence，得出safe和unsafe刚开始回答是diverge大，之后小
    - Attack原理很简单，一幅图就可以说清。就是用weak的unsafe和safe模型得出的token distribution来影响strong model
        
        ![Screenshot 2025-09-11 at 20.12.10.png](attachment:3e576df7-f0b9-44a4-b4de-6adeed3fd4ec:Screenshot_2025-09-11_at_20.12.10.png)
        
- An Interpretable N-gram Perplexity Threat Model  for Large Language Model **Jailbreaks**
    - 用了一个1trillion-token的n-gram model而不是llm model来派别perplexity
    - attack方法：所以adapt每个attack让prompt的perplexity都降低，发现adaptive后ASR（Attack Success Rate）变高
- SPEAK EASY: Eliciting Harmful **Jailbreaks** from LLMs with Simple Interactions
    - 提出了一个新的metric：HarmScore。单纯ASR（Attack Success Rate）无法衡量harmful response是否useful。通过actionable和informability等metric来具体衡量
    - 通过和model等interaction来慢慢induce有害回答
- FlipAttack: **Jailbreak** LLMs via Flipping
    - 很简单，就是通过不同flip的方法flip prompt
