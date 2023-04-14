# ChatDr
This is a chat bot to answer questions about a patients' medical history. To use the model for inference, use the ChatDr_v1 file.
In the (current) 1.0 version I attempted to use a generative approach by fine-tuning gpt-2 (schedule trained on a local GPU with early stopping) on some patient history medical records (the code is commented to explain each step). The training succeeded in that it produces medical sounding text related to the prompt (usually about the condition for example). The training metrics were monitored using TensorBoard (image below). The model weights and configuration files are available upon request (too large to upload here).

![Screenshot](TB_dashboard_ChatDr_v1.png "ChatDr_v1 Training Metrics")

However it doesn't produce the relevant info related to the patient from the prompt.
In the 2.0 version I take an extractive question answering approach, where given patient record/history as context, ChatDr will output the relevant answer from the context itself. The main challenge here was procuring a medical extractive QA dataset, I'm requesting access for an appropriate patient medical record dataset from Harvard School of Medicine.
In the meantime I used a model fine-tuned on SQuAD, then further finetuned specifically for scientific questions. The scientific QA dataset contained long context (longer than distilBERT max context size) in which case the context trianing data was sliced, and some of the contexts didn't contain the relevant answer, so this should serve to train the model to not answer questions if they are not in the context (similar to the SQuAD_v2 dataset).
Similar to thye 1.0 version this was also about to start overfitting but early stopping halted the training.
This approach is definitely better than 1.0 version, as it works. It does seem to struggle with some types of questions, and overall a model just trained on SQuAD_v2 (like 'deepset/minilm-uncased-squad2' on Hugging Face) seems to output higher quality answers.
I think the solution to move past these shortcomings is a high quality medical dataset to fine-tune it further on this specific task. As I still haven't found a dataset like this I might create one manually, by coming up with the questions and the answers from some medical records as contexts (not too large, maybe 1000 questions or so).

![Screenshot](TB_dashboard_ChatDr_v2.png  "ChatDr_v2 Training Metrics")
