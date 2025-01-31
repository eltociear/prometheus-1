---
layout: default
---

How can you evaluate whether your LLM is humorous or not? Among various versions during development, how can you track whether your LLM is inspiring while being culturally sensitive?

{: .sys-img}
![fine_grained_eval.](/assets/img/figure1.png)

Current evaluation resources (e.g., MMLU, Big Bench, AlpacaFarm) are confined to generic, single-dimensional evaluation metrics that are either too domain/task specific (e.g., EM, Rouge) or coarse-grained (e.g., helpfulness/harmlessness). To overcome this issue, recent work has introduced **Fine-grained Evaluation** (e.g., VicunaBench, MTBench, Flask), which measures a LLM's performance based on diverse skill sets (e.g., Creativity, Writing Ability, Role Playing Ability, Logical Abilitiy) based on GPT-4 evaluation.

However, employing GPT-4 as an evaluator LM has the following disadvantages:
* <b>Close-source Nature</b>: The proprietary nature of LLMs brings transparency concerns as internal workings are not disclosed to the broader academic community.
* <b>Uncontrolled Versioning</b>: As reproducibility is a cornerstone of scientific inquiry, any inconsistency stemming from version changes can undermine the robustness of research findings that depend on specific versions of the evaluator LM.
* <b>Prohibitive Costs</b>: Evaluating 4 LLM variants across four sizes (ranging from 7B to 65B) using GPT-4 on 1K evaluation instances can cost over $2000, which is prohibitive for academic institutions.

<br>
To this end, we introduce <span class="sys-name">**Prometheus**</span> 🔥, a fully open-source LLM (7B & 13B) that shows high correlation with both human evaluators and GPT-4!

<br>
<br>
## Inducing Fine-grained Evaluation Capability
The main obstacle of obtaining a language model specialized on evaluation is because it needs to **know the important aspects tailored with the instruction** and should be able to **internally estimate what the answer of the instruction might be** in the first place. After then, the evaluator LM could assess the quality of the responses based on the information derived from the previous two steps.

Our main intuition is that by incorporating the appropriate reference materials, the evaluator LM could solely focus on assessing the quality of the response instead of determining the important aspects or solving the instruction itself.

{: .sys-img}
![input_output_format.](/assets/img/figure2.png)

Specifically, we append a **Score Rubric** and a **Reference Answer** for the following purpose:
* <b>Score Rubric</b>: Provides information of the pivotal aspects essential for addressing the given instruction. Without it, the evaluator LM should inherently know what details should be considered from the given instruction.
* <b>Reference Answer</b>: Relieves the need for the evaluator LM to actually solve the given instruction and solely focus on evaluation the response. This enables to bypass a natural proposition that if an evaluator LM doesn't have the ability to solve the problem, it is likely that it cannot evaluate different responses effectively as well.

<br>
<br>
## The Feedback Collection Dataset
Along with the model, we release the **Feedback Collection**, which is a new feedback dataset that was used to train <span class="sys-name">**Prometheus**</span> 🔥!

Compared to previous feedback datasets (e.g., Selfee, Shepherd), the **Feedback Collection** consists of 1K fine-grained score rubrics, 20K instructions, and 100K responses and language feedback generated by GPT-4.
</br>

The construction process is consisted of (1) obtaining 50 seed score rubrics from human annotators, (2) expanding the score rubrics with brainstorming and paraphrasing, (3) obtaining instructions closely related to the score rubric, and (4) acquiring the remaining components for each instance.

{: .sys-img}
![Animation of the overall workflow of EvalLM where users sample inputs from a dataset, generate outputs from each input using two different prompts, and then comparatively evaluate these outputs on user-defined criteria.](/assets/img/animation.gif)

The main considerations while constructing the **Feedback Collection** were:
* Including as many reference materials (score rubric, reference answer) as possbile.
* Maintaining a uniform length among the reference answers for each score (1 to 5) to prevent undesired length bias.
* Maintaining a uniform score distribution among responses to prevent undesired decision bias after fine-tuning.
* Limiting the scope of the score rubrics and instructions to realistic situations where a user is interacting with a LLM.

------

For more information about our work, please check out our paper! Also, we plan to continually update our model based on your feedback! Feel free to reach out to us via email or twitter!

## Bibtex

<pre>
@misc{kim2023prometheus,
      title={Prometheus: Inducing Fine-grained Evaluation Capability in Language Models}, 
      author={Seungone Kim and Jamin Shin and Yejin Cho and Joel Jang and Shayne Longpre and Hwaran Lee and Sangdoo Yun and Seongjin Shin and Sungdong Kim and James Thorne and Minjoon Seo},
      year={2023},
      eprint={2310.08491},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
</pre>

------

{: .logos}
[![Logo of LKLab](/assets/img/lklab_logo.png)](https://lklab.kaist.ac.kr/)
[![Logo of KAIST](/assets/img/kaist_logo.png)](https://kaist.ac.kr)
[![Logo of NAVER](/assets/img/naver_ai_lab_logo.png)](https://www.facebook.com/NAVERAILAB)

{: .center .acknowledgement}
This research was supported by the **KAIST-NAVER Hypercreative AI Center**.
