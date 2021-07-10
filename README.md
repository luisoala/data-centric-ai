# Data Centric AI _(v0.0.2)_

We're collecting (an admittedly opinionated) list of resources and progress made
in data-centric AI, with exciting directions past and present.

While AI has been pretty focused on models, the real-world experience of those who put
models into production is that the data often matters more. The goal of this repository
is to consolidate this experience in a single place that can be accessed by anyone that
wants to understand and contribute to this area.


## How Can I Help?

If you're interested in this area and would like to hear more, join our [mailing list](https://groups.google.com/forum/#!forum/data-centric-ai/join)! We'd also appreciate if you could fill out this short [form](https://docs.google.com/forms/d/e/1FAIpQLSf5UcTJnvMIcLzxvTgac5Jdvyry3u2XsewMrXFosgKtWTTGxA/viewform?usp=sf_link)
to help us better understand what your interests might be.

### Feedback (Interested in a class?)
We are creating a class at Stanford about data-centric AI, and we'd love your feedback. If you are interested in learning more, please fill out this [form](https://docs.google.com/forms/d/e/1FAIpQLSf5UcTJnvMIcLzxvTgac5Jdvyry3u2XsewMrXFosgKtWTTGxA/viewform?usp=sf_link).

### Contributing

We want this resource to grow with contributions from readers and data enthusiasts.
Make a pull request if you want to add resources.

#### Instructions for editing existing area pages

- For making edits to existing areas and area pages, submit a PR containing the relevant changes.
- We welcome changes
  such as missed subtopics, clarifying discussion, additional resources and links such as citations, paper links, blogs, videos, code, workshops,
  classes, tutorials, figures, pictures, recipes, tweets and books.

#### Instructions for adding new areas

If you'd like to add a new area page, use the [Area Page Issue](https://github.com/HazyResearch/data-centric-ai/issues/new/choose) to suggest a new area: your request will need to be approved by admins before you make any PRs that add a new area page.

- If writing a new area page, write a summary for the area and how it relates to data-centric AI in README.md.
- Add a new area page. In the area page, add subheaders for different subtopics that might be important.
  Add a few critical links (citations, paper links, blogs, videos, code, workshops,
  classes, tutorials, figures, pictures, recipes, tweets, books), and a short
  description for each sub-topic and how it relates to the section.

#### General instructions

- Use `h1` or `h2` headers for all major headings (other subheadings can be in standard markdown).
  So, use `<h1 id="sec:my-h1-section"></h1>` or `<h2 id="subsec:my-h2-section"></h2>` to tag sections
  instead of standard markdown hashes.

---

# Table of Contents

1. [Data Programming & Weak Supervision](#data-programming)
2. [Data Augmentation](#augmentation)
3. [Self-Supervision](#self-supervision)
4. [The End of Modelitis](#end_modelitis)
5. [Fine-Grained Evaluation](#evaluation)
6. [Robustness](#robustness)
7. [Emerging Trends](#emerging)
8. [Applications](#applications)
9. [Workshops](workshops.md)

<h1 id="data-programming">Data Programming & Weak Supervision</h1>

[Data Programming & Weak Supervision Area Page](data-programming.md)

Many modern machine learning systems require large, labeled datasets to be successful but producing such datasets is time-consuming and expensive. Instead, weaker sources of supervision, such as [crowdsourcing](https://papers.nips.cc/paper/2011/file/c667d53acd899a97a85de0c201ba99be-Paper.pdf), [distant supervision](https://www.aclweb.org/anthology/P09-1113.pdf), and domain experts' heuristics like [Hearst Patterns](https://people.ischool.berkeley.edu/~hearst/papers/coling92.pdf) have been used since the 90s.

However, these were largely regarded by AI and AI/ML folks as ad hoc or isolated techniques. The effort to unify and combine these into a data centric viewpoint started in earnest with [data programming](https://arxiv.org/pdf/1605.07723.pdf) embodied in the [Snorkel system](http://www.vldb.org/pvldb/vol11/p269-ratner.pdf), now an [open-source project](http://snorkel.org) and [thriving company](http://snorkel.ai). In Snorkel's conception, users specify multiple labeling functions that each represent a noisy estimate of the ground-truth label. Because these labeling functions vary in accuracy, coverage of the dataset, and may even be correlated, they are combined and denoised via a latent variable graphical model. The technical challenge is thus to learn accuracy and correlation parameters in this model, and to use them to infer the true label to be used for downstream tasks.

Data programming builds on a long line of work on parameter estimation in latent variable graphical models. Concretely, a generative model for the joint distribution of labeling functions and the unobserved (latent) true label is learned. This label model permits aggregation of diverse sources of signal, while allowing them to have varying accuracies and potential correlations.

An overview of the weak supervision pipeline can be found in this [Snorkel blog post](https://www.snorkel.org/blog/weak-supervision), including how it compares to other approaches to get more labeled data and the technical modeling challenges. These [Stanford CS229 lecture notes](https://mayeechen.github.io/files/wslecturenotes.pdf) provide a theoretical summary of how graphical models are used in weak supervision.

<h1 id="augmentation">Data Augmentation</h1>

[Data Augmentation Area Page](augmentation.md)

A key challenge when training machine learning models is collecting a large, diverse dataset that sufficiently captures the variability observed in the real world. Due to the cost of collecting and labeling datasets, data augmentation has emerged as a cheap, promising alternative.

The central idea in data augmentation is to transform examples in an existing dataset to generate additional augmented examples that can then be added to the dataset. These additional examples typically increase the diversity of the data seen by the model, and provide additional supervision to the model. The foundations of data augmentation originate in [tangent propagation](https://papers.nips.cc/paper/1991/file/65658fde58ab3c2b6e5132a39fae7cb9-Paper.pdf), which introduced techniques to make a learned model invariant with respect to some transformation of the data.

Early successes in augmentation such as [AlexNet](https://papers.nips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf) focused on inducing invariances in an image classifier by generating examples that encouraged translational or rotational invariance. These successes made augmentation a de-facto part of pipelines for a wide-ranging set of tasks such as image, speech and text classification, machine translation, etc.

The choice of transformations used in augmentation is an important consideration, since it dictates the invariances learned by the model, and its behavior when encountering a diversity of test examples. While heuristic augmentations have remained popular, it is important to be able to control and program the augmentation pipeline more carefully. [TANDA](https://arxiv.org/pdf/1709.01643.pdf.) initiated a study of the problem of programming augmentation pipelines by composing a selection of data transformations. This area has since seen rapid growth with both deeper theoretical understanding and practical implementations such as [AutoAugment](https://openaccess.thecvf.com/content_CVPR_2019/papers/Cubuk_AutoAugment_Learning_Augmentation_Strategies_From_Data_CVPR_2019_paper.pdf). A nascent line of work has leveraged conditional generative models to learn-rather than specify-these transformations, further extending this programming paradigm.

<h1 id="self-supervision">Self-Supervision</h1>

[Self-Supervision Area Page](self-supervision.md)

The need for large, labeled datasets has motivated methods to pre-train latent representations of the input space using unlabeled data and use the now knowledge-rich representations in downstream tasks. As the representations allow for knowledge transfer to downstream tasks, these tasks require less labeled data. For example, language models can be pre-trained to predict the next token in a textual input to learn representations of words or sub-tokens. These word representations are then used in downstream models such as sentiment classification. This paradigm, called "self-supervision", has revolutionized how we train (and pre-train) models. Importantly, these self-supervised pre-trained models learn without manual labels or hand curated features. This reduces the engineer effort to create and maintain features and makes models significantly easier to deploy and maintain. This shift has allowed for more data to be fed to the model and shifted the focus to understanding what data to use.

<h1 id="end_modelitis">The End of Modelitis</h1>

[The End of Modelitis Area Page](end-of-modelitis.md)

Historically, the "kid in a candy shop" moment for ML researchers is building and tweaking models using tools like PyTorch or Jax. New models were coming out each day and these customize model architectures and finely-tuned parameters were beating state-of-the-art results. This modelitis craze, however, is coming to an end.

Recently, researchers have realized two things: (1) more gains are coming from deeply understanding the data rather than model tweaking (see all the exciting work in Data Augmentation), and (2) custom models are difficult to maintain and extend in a production environment. This resulted in model building platforms like [Ludwig](https://eng.uber.com/introducing-ludwig/) and [Overton](https://www.cs.stanford.edu/~chrismre/papers/overton-tr.pdf) that enforced commoditized architectures. And they showed these commoditiy models were even better than their tuned predecessors! This result was further supported by [Kaplan et al](https://arxiv.org/pdf/2001.08361.pdf) that showed the architecture matters less than the data.

This trend, which we are calling the End of Modelitis, is moving towards a data-centric view of model construction. The question is shifting from “how to construct the best model” to “how do you feed a models”.

<h1 id="evaluation">Evaluation</h1>

[Evaluation Area Page](evaluation.md)

Model evaluation is a crucial part of the model development process in machine learning. The goal of evaluation is to understand the quality of a model, and anticipate if it will perform well in the future.

While evaluation is a classical problem in machine learning, data-centric AI approaches have catalyzed a shift towards _fine-grained evaluation_: moving beyond standard measures of average performance such as accuracy and F1 scores, to measuring performance on particular populations of interest. This enables a more granular understanding of model performance, and gives users a clearer idea of model capabilities. This shift is complentary to a growing interest in understanding model robustness, since access to fine-grained evaluation permits an enhanced ability to build more robust models.

Approaches to fine-grained evaluation include measuring performance on critical data subsets called slices, invariance or sensitivity to data transformations, and resistance to adversarial perturbations. While most evaluation is user-specified, an important line of work found that models often underperform on _hidden strata_ that are missed by model builders in evaluation, which can have profound consequences on our ability to deploy and use models. This motivates future work in automatically discovering these hidden strata, or more generally, finding all possible failure modes of a model by analyzing datasets and models in systematically in conjunction.

Another important facet of fine-grained evaluation is data and model monitoring in order to anticipate, measure and mitigate degradations in performance due to distribution shift. This includes identifying and isolating data points that may be considered outliers, estimating performance on unlabeled data that is streaming to a deployed model, and generating rich summaries of how the data distribution may be shifting over time.

<h1 id="robustness">Robustness</h1>

[Robustness Area Page](robustness.md)

One standard assumption for successfully deploying machine learning models is that test time distributions are similar to those encountered and well-represented during training. In reality however, this assumption rarely holds: seldom do we expect to deploy models in settings that exactly match their training distributions. Training models robust to distribution shifts is then another core challenge to improve machine learning in the wild, which we argue can be addressed under a data-centric paradigm.

Here, we broadly categorize attempts to improve robustness to distribution shifts as those addressing (1) subpopulation shift or hidden stratification, (2) domain shift, and (3) shifts from adversarial perturbations.

Under subpopulation shift, training and test-time distributions differ in how well-represented each subpopulation or “data group” is. If certain subpopulations are underrepresented in the training data, then even if these distributions are encountered during training, standard empirical risk minimization (ERM) and “learning from statistical averages” can result in models that only perform well on the overrepresented subpopulations.

- One initial real world problem under subpopulation shift came with training models on datasets that exhibit spurious correlations. If a majority of groups exhibit relations between certain features and the target of interest, but these dependencies do not hold for all data, then models may learn non-robust dependencies relying on these “spurious” correlations. If these groups are known, [Group DRO](https://arxiv.org/abs/1911.08731) to prevent this by focusing optimization on worst-group error.
- Another instantiation comes from hidden stratification, where datapoints belonging to the same labeled classes can actually vary in their feature distributions quite a bit. With [GEORGE](https://arxiv.org/abs/2011.12945), we learned that despite not being able to generalize to unseen data under all groups, deep neural networks trained with ERM can actually learn separable representations for different groups that share the same label.

Both Group DRO and GEORGE introduced approaches to handle subpopulation shift under real-world instantiations. These methods have inspired additional work related to upsampling estimated groups ([LfF](https://arxiv.org/abs/2007.02561), [JTT](link_here)) and using contrastive learning to learn group-invariant representations ([CNC](link_here)).

Beyond subpopulation shift, robustness also features domain shift and adversarial perturbations. Under domain shift, we model test-time data as coming from a completely different domain from the training data. Under distribution shift with adversarial perturbations, test-time data may exhibit corruptions or imperceptible differences in input feature space that prevent trained ERM models from strongly generalizing to the test-time distributions. _These sections are stubs - review if they should be in-scope here._

<h1 id="emerging">Emerging Trends</h1>

[Emerging Trends Area Page](emerging.md)

Data-centric AI is still growing, and we want to capture emerging trends as they arise. Some new areas we think are forming involve interactive machine learning, massice scale models, and observational ML. Take a look at the area page.

<h1 id="applications">Applications</h1>

[Applications Area Page](applications.md)

Data-centric approaches have had a wide-ranging impact wherever machine learning is used and deployed, whether in academia, industry or other organizations. Impact spans modalities such as structured data, text, images, videos, graphs and others, while areas include text and image processing, medical imaging, computational biology, autonomous driving, etc.
