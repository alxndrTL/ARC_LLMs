# ARC_LLMs : Keeping track of LLM progress on ARC
 
 *Head over to [alxndrtl.github.io/ARC](https://alxndrtl.github.io/ARC/) if you want to visualize the ARC tasks solved (or not) by LLMs !*
 
![example_task](example_task.png)

 
 This repo contains the code and results for the evaluation of some famous LLMs on the [Abstraction and Reasoning Corpus](https://github.com/fchollet/ARC).
 
 In the `responses` folder, there is one folder per model, with the completions of all the tasks it was evaluated on (as long as it answered with a valid response - ie one that can be made into a numpy array). The file `results.txt` shows on which specific tasks the different models succeeded. The code used to get the results is also available.
 
 The present document shows the results, then elaborates on why it matters and why this is interesting for present and future research, and finally, touch the point that while the ARC tasks were seen during training, there is little evidence, following multiple experiment, that the model just learned the tasks.

 The code used to get the results is also provided as a notebook. Note that it is compatible only with the 0.27 version of the python openai library. Please see the OpenAI documentation if you want to work with newer versions (very little changes).
 
 ## Results
 
 The direct evaluation of ARC tasks on LLMs yields the following results :
 
<div align="center">
 
 | model                  | result |
|------------------------|--------|
| gpt-4                  | 21%    |
| gpt-4-turbo            | 18%    |
| text-davinci-003       | 14%    |
| gpt-3.5-turbo-instruct | 10.5%  |
| gpt-3.5-turbo (4k)         | 11%  |
| text-davinci-002       | 10.5%  |
| llama2-70b             | 4%     |
| llama2-70b-chat        | 0%     |

</div>

Note : these are pass@1 results. The ARC paper suggests to leave 3 tries to the human/model. This is not done here, only one try. 

All the models were evaluated on the same subset composed of 100 tasks choosen randomly (from the 400 training tasks), except for :
- text-davinci-002, evaluated on 200 of these 400 training tasks (choosen randomly).
- text-davinci-003 and gpt-3.5-turbo-instruct, evaluated on the whole 400 training tasks.
Default temperature was used (=1), except for gpt-3.5-turbo-instruct, gpt-3.5-turbo and gpt-4-turbo, where a temperature of 0 was used.

Note : some tasks were too long to fit in the context of some models (namely : text-davinci-003/2, and the llamas), but the percentage shown here doesn't take it into account (basically, a task is considered failed if its too big to fit in the context). It doesn't really matter, as most task that are long are failed by these models (more about it below).

<div align="center">

 | model                  | mean lenght of succeeded tasks | context lenght |
|------------------------|--------|--- |
| gpt-4                  | 1066    | 8k |
| gpt-3.5-turbo-instruct | 600  | 4k |
| gpt-3.5-turbo (4k)         | 553  | 4k|

</div>

The mean length of the tasks which the models were evaluated on is <b>2242</b>. So we see that the LLMs mostly succeed on short tasks. Is it because these tasks are easier ? or is it because their context-learning ability is limited to short contexts ? A little bit of both I would say. The gap between the 3.5 and 4 family is impressive.


## Why it matters ?

I think that this benchmark is very interesting for the fine-tuning of LLMs. We see that turning an LLM into a chatbot with RLHF makes the success rate goes down by a few points. Inversly, can't we fine-tune an LLM in an other way, and have it perform better on ARC ? Of course, the final goal isn't to have a model good at ARC. As I said, training on the ARC tasks is I believe irrelevant. But a chatbot is just one possiblity among many other when one chooses what to do with a base LLM. See [Grounding Large Language Models in Interactive Environments with Online Reinforcement Learning](https://arxiv.org/abs/2302.02662) for an example of such fine-tuning.

## To-Do
Focus the test on smaller models and see the impact of different training/fine-tuning choices.
For example, compare llama-7b with its code variants. Try phi-1.5 (first results aren't good, about 1%).
