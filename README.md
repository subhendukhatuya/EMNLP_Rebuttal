#### **Reviewer TBVF**

**Paper Summary:**  
This paper presents FINDER for numerical reasoning. FINDER is a retriever-generator framework, where the retriever is the fine-tuned FLAN-T5 Large model, and the generator is GPT-4 with POT prompting on diverse in-context examples. FINDER achieves SOTA performance on two datasets. Although this paper has improved significantly over its previous version, my major concern is regarding the novelty of FINDER.

**Summary Of Strengths:**

1. Clear presentations  
2. SOTA performance

**Summary Of Weaknesses:**

1. Limited novelty. Most components are combinations of existing methods.  
2. Unfair experimental comparison. FINDER is based on GPT-4. I would like to see more baselines with GPT-4.

**Comments Suggestions And Typos:**  
See weakness

**Confidence:** 4 \= Quite sure. I tried to check the important points carefully. It's unlikely, though conceivable, that I missed something that should affect my ratings.

**Soundness:** 4 \= Strong: This study provides sufficient support for all of its claims. Some extra experiments could be nice, but not essential.

**Excitement:** 2 \= Potentially Interesting: this paper does not resonate with me, but it might with others in the \*ACL community.

**Overall Assessment:** 2.5 \= Borderline Findings

**Rebuttal Response:**  
 

Please find below our responses to the specific comments you made:-

Limited novelty. Most components are combinations of existing methods.

\*\*Response:\*\*    
While our framework builds on ideas like dynamic in-context learning and PoT prompting, the novelty lies in the **specific refinements and task-adapted enhancements** we introduce in each module to push performance beyond standard approaches.

**Enhancements in the Target Computation module:**

1) Diverse candidate questions: Instead of relying on random or static examples, we cluster training questions and select representatives from each cluster. This ensures better coverage of reasoning patterns while keeping the prompt concise (Section 3.2.2).  
2) Reward computation:  Unlike prior methods that directly match GPT-4’s generated answers, we use PoT-style prompting to generate code, execute it to get the final answer, and score examples based on their reasoning contribution. This avoids unfairly penalizing examples when GPT-4 makes arithmetic errors (lines 303–314).  
3) Strategic ordering**:** We order higher-scoring in-context examples closer to the question, which empirically improves accuracy by 1.64% on FinQA and 1.47% on ConvFinQA (Table 5).

**Enhancements in the Retriever module:**

1) We use an instruction-tuned LLM instead of scoring-based retrievers, making selection more flexible than choosing a fixed number of lines (Section 3.2.1).

2) This design achieves over a 99% reduction in parameters compared to SOTA retrievers, as shown in Table 3, while improving or maintaining performance.

These refinements are not just integration—they are purposeful, task-driven modifications that together form a pipeline which significantly improves accuracy and generalizes well to other LLMs, including Gemini 2.0, GPT-3.5 Turbo (Section 8.3). As explained in lines 72–82, prior methods did not combine a retriever–generator framework with advanced prompting strategies, leaving this integration unexplored.

In summary, our contribution lies in the design and empirical validation of these targeted enhancements, rather than reusing standard components without adaptation.

Unfair experimental comparison. FINDER is based on GPT-4. I would like to see more baselines with GPT-4.

\*\*Response:\*\*  

Thank you for your comment. We agree that GPT-4 is a very strong model; however, as shown in Table-2, simply using GPT-4 alone or with existing prompting methods like POT, COT, or TabT5 does **not** surpass the task-specific SOTA model APOLLO. Our method outperforms these GPT-4 baselines by combining GPT-4 with relevant retrieved facts and dynamic in-context examples.

In Section 8.1, we further demonstrate this by replacing our retriever with APOLLO’s retrieved facts but keeping the same GPT-4 target computation module: while this improves accuracy over APOLLO (by 2.67% and 2.42% on FinQA and ConvFinQA), it still falls short of our full retriever \+ target computation approach.

Additionally, in Section 8.2, we include a static GPT-4 baseline where all test samples get the same in-context examples. Our dynamic prompt selection framework, which uses GPT-4 with a diverse set of examples, improves over this static baseline by 5.08% and 4.6% on FinQA and ConvFinQA.

These results highlight that the improvement is not from GPT-4 alone, but from our framework’s design—specifically, the dynamic prompt selection and retrieval strategy that extract stronger performance from GPT-4.

#### **—--------------------------------------------------------------------------------------------**

#### 

#### **Reviewer ohhs**

**Paper Summary:**  
In this paper, they explore the use of Program of Thoughts on 2 financial datasets FinQA and ConvFinQA. The FINDER framework divides the task into generative retrieval and dynamic selection of in-context examples. The paper shows improved performance over current benchmarks.

**Summary Of Strengths:**  
Overall, this paper was well-motivated and well-written. The authors provide good rationale for their model design choices and they achieved good performance. The use of dynamic in-context examples is a nice technique for this problem. Additionally, I appreciated the strong evaluation including an ablation study and error analysis.

**Summary Of Weaknesses:**

1. Figure 3 doesn't add much to the paper as it is unclear what the concepts the clusters represent and how that subset was selected for visualization. Were the most well defined clusters selected or were they randomly chosen?  
2. Although the paper is framed as addressing issues in finance and the dataset and examples were financial in nature, it doesn't seem to me that there was anything about the FINDER framework that is specific to finance. The approach looks generally applicable to numerical reasoning tasks across domains (which is good), so the exclusive focus on financial datasets doesn't feel as well motivated.

**Comments Suggestions And Typos:**  
Lines 201 and 502: LLM's \--\> LLMs

**Confidence:** 4 \= Quite sure. I tried to check the important points carefully. It's unlikely, though conceivable, that I missed something that should affect my ratings.

**Soundness:** 4.5

**Excitement:** 4.0 \= Exciting: I would mention this paper to others and/or make an effort to attend its presentation in a conference.

**Overall Assessment:** 4.0 \= Conference: I think this paper could be accepted to an \*ACL conference.

Figure 3 doesn't add much to the paper as it is unclear what the concepts the clusters represent and how that subset was selected for visualization. Were the most well defined clusters selected or were they randomly chosen?

\*\*Response:\*\*  

Thank you for the feedback. We agree that the purpose of Figure 3 can be made clearer. The t-SNE plot is intended to provide an intuitive visualization of the semantic distribution of few clusters formed using Sentence-BERT embeddings. Due to space constraint, we selected a subset of clusters—such as those related to growth rates, amortization, and ROI—to demonstrate that our clustering captures distinct financial reasoning concepts.

This is further supported in Table 1, where we present representative questions for each cluster and explicitly highlight the core financial concepts in bold. These concepts align with the themes visible in the t-SNE clusters. Together, Figure 3 and Table 1 help illustrate how clustering contributes to selecting semantically diverse and thematically grounded in-context examples.

In the final version, we will improve the caption and narrative around Figure 3 to better communicate the rationale behind the selected clusters and their interpretability.

Although the paper is framed as addressing issues in finance and the dataset and examples were financial in nature, it doesn't seem to me that there was anything about the FINDER framework that is specific to finance. The approach looks generally applicable to numerical reasoning tasks across domains (which is good), so the exclusive focus on financial datasets doesn't feel as well motivated.

\*\*Response:\*\* 

Thank you for the insightful comment. You are correct that the FINDER framework itself is not inherently limited to finance and is generally applicable to numerical reasoning tasks across domains.

We chose to focus on financial datasets for several reasons. First, as highlighted in lines 39–47, LLM-based solutions currently tend to underperform compared to task-specific architectures like APOLLO. Second, the financial domain brings unique challenges—including reasoning over both structured tables and unstructured text, handling multi-step numerical computations, and addressing domain-specific concepts such as growth rates and amortization. Evaluating on FinQA and ConvFinQA, which are established benchmarks for financial reasoning, allows us to directly measure these improvements in a high-impact, real-world setting.

Our goal was to demonstrate that an LLM-based pipeline can match or outperform specialized models within this challenging domain. As noted in the conclusion (lines 540–542), we also plan to extend FINDER to other domains that require complex numerical analysis and to explore integrating external knowledge sources—further highlighting its broader applicability beyond finance.

**Comments Suggestions And Typos:** Lines 201 and 502: LLM's \--\> LLMs  
\*\*Response:\*\*   
Thank you for pointing this out. We will correct “LLM's” to “LLMs” in Lines 201 and 502 in the final version.

#### **—--------------------------------------------------------------------------------------------**

#### **Reviewer 8jar**

**Paper Summary:**  
This paper proposes a novel two-stage framework FINDER. FINDER first extracts facts relevant to a question from unstructured data containing text and tables using the instructional fine-tuned generative searcher FLAN-T5. It then generates an executable program using GPT-4 by introducing dynamically selected contextual examples combined with PoT hints, which is symbolically calculator to execute to obtain the final answer. The framework achieves new optimal performance on both FinQA and ConvFinQA benchmark datasets, improving execution accuracy by 5.98% and 4.05%, respectively, compared to traditional static examples and scoring-based retrieval methods. The authors also optimized the example selection process using clustering techniques, diversification strategies, and reinforcement learning algorithms, which significantly enhanced the model's generalization ability and numerical inference accuracy.

**Summary Of Strengths:**

1. The FLAN-T5 model fine-tuned with LoRA provides efficient parameter usage and higher relevance retrieval, which significantly outperforms traditional scored retrievers such as APOLLO.  
2. Improved PromptPG strategy introduces problem clustering and reinforcement learning algorithms, which ensures diversity of examples and contextual relevance and effectively improves reasoning Quality.  
3. The proposed method combines PoT hints and GPT-4 to generate Python code, and then obtaines the answer through symbolic executors, which enhances the model's ability to deal with multi-step logic and financial computation.

**Summary Of Weaknesses:**  
The clustering section mentions the use of Sentence BERT and agglomerative clustering methods for clustering the problems and selecting representative problems for each cluster. However, specific descriptions of the details of the clustering techniques, parameter selection, and how to determine the number of clusters (e.g., 50 clusters) are still sketchy, and the logic and empirical rationale behind these settings are not fully described.

**Comments Suggestions And Typos:**

1. Some of the formulas in sections 3.2.2 and 3.3.1 are not ordinalized and lack formula numbers.  
2. Although the authors mention that 50 clusters were selected, there is no further elaboration on the basis for the selection of this number. There should be further explanation of how the number of clusters was determined and whether some method was used to validate the reasonableness of this number, such as assessing the quality of the clusters through metrics such as Silhouette Score, or an empirical choice based on the distribution of the data.  
3. Errors are categorized in the appendix (fact retrieval errors, ground truth or question issues, and logical errors), but the root causes of each type of error are not analyzed in depth. The authors could consider further refining the error analysis to explore the specific scenarios in which different types of errors occur and propose targeted improvement measures.

**Confidence:** 3 \=  Pretty sure, but there's a chance I missed something. Although I have a good feel for this area in general, I did not carefully check the paper's details, e.g., the math or experimental design.  
**Soundness:** 3.5  
**Excitement:** 2.5  
**Overall Assessment:** 3 \= Findings: I think this paper could be accepted to the Findings of the ACL.

The clustering section mentions the use of Sentence BERT and agglomerative clustering methods for clustering the problems and selecting representative problems for each cluster. However, specific descriptions of the details of the clustering techniques, parameter selection, and how to determine the number of clusters (e.g., 50 clusters) are still sketchy, and the logic and empirical rationale behind these settings are not fully described.

\*\*Response:\*\*    
Thank you for the valuable feedback. We acknowledge that the current version lacks detailed justification for our clustering setup. As mentioned in lines 263–264, we initially compared Sentence-BERT embeddings with TF-IDF and observed that TF-IDF did not yield good clustering results. To further validate this choice, we have now additionally experimented with other sentence embedding models, including Universal Sentence Encoder and MPNet. The silhouette scores for these variants are shown in Table below. Sentence-BERT continues to outperform these alternatives, supporting its use. We will include these additional results and details in the final version for clarity.

| Embedding Model | Silhouette Score |
| ----- | ----- |
| TF-IDF | 0.11 |
| Universal Sentence Encoder | 0.23 |
| MPNet | 0.26 |
| **Sentence-BERT (ours)** | **0.34** |

For clustering, we used agglomerative clustering with average linkage, and selected 50 clusters empirically by testing values in the range of 30–70 and choosing the one that provided a good trade-off between cluster coherence and diversity.   
The majority of distinct financial concepts—such as growth rates, amortization, percentage change, ROI, etc.—could be meaningfully grouped within this number. In the final version, we will include these details, along with the clustering parameters and evaluation process.

Some of the formulas in sections 3.2.2 and 3.3.1 are not ordinalized and lack formula numbers.

\*\*Response:\*\*  

Thank you for pointing that out. In Section 3.2.2, the expression Si=IP ∣∣ TabTexti ∣∣ Di ∣∣ Qi was intended as a structural input format illustration rather than a formal equation. In the final version, we will format them as inline text to avoid confusion.

For Section 3.3.1, we acknowledge the missing equation number for reward computation and will revise the final version to properly number and format all formulas for consistency and clarity.

Although the authors mention that 50 clusters were selected, there is no further elaboration on the basis for the selection of this number. There should be further explanation of how the number of clusters was determined and whether some method was used to validate the reasonableness of this number, such as assessing the quality of the clusters through metrics such as Silhouette Score, or an empirical choice based on the distribution of the data.

\*\*Response:\*\*  

Thank you for the insightful comment. We agree that further clarification on the selection of the number of clusters would strengthen the section. As noted in Lines 261–263, we did mention that Sentence-BERT yielded the best clusters based on higher Silhouette Scores. 

However, we acknowledge that we did not explicitly explain how the number of clusters (50) was chosen. Actually, we evaluated cluster quality across a range of values (30 to 70\) using the Silhouette Score and found that 50 provided the best balance between semantic cohesion and diversity. Additionally, we empirically observed that the key financial concepts in our dataset—such as growth rates, amortization, percentage change, ROI, etc.—could be well captured within 50 clusters. We will include these details in the final version for greater clarity and justification.

Errors are categorized in the appendix (fact retrieval errors, ground truth or question issues, and logical errors), but the root causes of each type of error are not analyzed in depth. The authors could consider further refining the error analysis to explore the specific scenarios in which different types of errors occur and propose targeted improvement measures.

\*\*Response:\*\*  

Thank you for the helpful feedback. While we categorized errors and included representative examples in Appendix A.1, we acknowledge that the root causes for each error type were not explicitly analyzed or structured in the current version of the paper. We mentioned some high-level contributing factors—such as flawed reasoning and incorrect computations—and briefly suggested mitigation strategies (Lines 904–907), including improvements to program generation, dataset annotation, and the use of domain-specific knowledge.

To clarify further:

1. For **fact retrieval errors**, we identified the model’s limited temporal and numerical awareness as a key issue. Instruction-tuned FLAN-T5 sometimes fails to retrieve facts aligned with the correct fiscal periods or quantitative cues, which are essential in financial documents.  
2. For **logical errors**, even with correct facts retrieved, GPT-4 with Program-of-Thought prompting via PromptPG occasionally generates incorrect answers due to misinterpretation of formats (e.g., percentages), or subtle bugs in the program. These stem from weak reward signals for format accuracy, lack of domain constraints, and reliance on surface-level patterns. Strengthening the reward function and incorporating domain-aware prompting could help mitigate these issues.  
3. For **ground truth or question issues**, errors typically result from incorrect or ambiguous labels in the dataset. A key improvement strategy here is careful data re-annotation to ensure consistency and correctness.

We will expand the appendix in the final version to explicitly list such root causes per error category and propose targeted improvement measures to address them.

#### **—--------------------------------------------------------------------------------------------**