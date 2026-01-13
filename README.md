# 🤖 Finetuned Chatbot

Welcome to the **Finetuned Chatbot** repository! 🚀 This project showcases a chatbot application built using Streamlit, leveraging the powerful `facebook/bart-base` model fine-tuned on the `Bitext-customer-support-llm-chatbot-training-dataset`. The primary goal? To test the conditional generation capabilities of BART while delivering contextually relevant and well-structured responses. 💬✨

---

## 📂 About the Fine-Tuned Model

You can find the fine-tuned model and related files<a href="https://drive.google.com/drive/folders/1dZXL4ucOjCkc2l2qSqOhIarS38ZGuD3Q?usp=drive_link" target="_blank"> here on Google Drive</a>. 💾✨ []().

### 📊 Model Metrics

Here are the key metrics from the fine-tuning process:

- **Runtime**: 15,521.44 seconds
- **Steps**: 110
- **Evaluation Loss**: 0.1015
- **Evaluation Runtime**: 411.59 seconds
- **Samples per Second**: 13.059
- **Steps per Second**: 1.088
- **Total FLOPs**: 39,322,513,928,355,840
- **Training Epochs**: 3
- **Global Steps**: 5376
- **Gradient Norm**: 0.3138
- **Learning Rate**: 9.67e-8
- **Training Loss**: 0.116
- **Overall Training Loss**: 0.2455
- **Training Runtime**: 15,513.66 seconds
- **Samples per Second**: 4.157
- **Steps per Second**: 0.347

---

## ⚙️ Model Used

The chatbot is powered by the `facebook/bart-base` model, fine-tuned for this specific use case. You can explore the fine-tuning process in detail through this <a href="https://github.com/Firojpaudel/GenAI-Chronicles/blob/main/Seq2Seq/BART_generator_finetuning.ipynb" target="_blank">Notebook implementation</a>. 📒✨

---

## 🏷️ About This Repository

This repository features a **basic Streamlit app** that brings the chatbot to life! While it’s still a work in progress, here’s what it currently offers:

- ✅ Contextually relevant responses
- ✅ Well-structured output based on the fine-tuned dataset

### 🎥 App Demo

Check out the demo in action:

<div style="text-align: center;">
  <img src="./README_Images/Chatbot_.gif" >
</div>

### 🌟 Future Enhancements

Well now I've added the conversational Memory and implemented **RAG** as well.

> However, its not that optimised and runtime is currently a bit high. Maybe we can reduce the inferencing time 🤷‍♂️

---

## 📚 Documentation

For detailed information about the project's architecture, database schema, and usage, please refer to the [Project Documentation](DOCUMENTATION.md).

---

## 📜 Licensing

This repository is licensed under the MIT License. 📝 Feel free to explore, use, and contribute!

---

## 🌈 Final Notes

This project was created as a test of BART’s conditional generation capabilities—and it delivered brilliantly! ✨ The model’s ability to generate structured responses opens up exciting possibilities for similar applications.

Stay tuned for more updates and enhancements!
