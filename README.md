# HerLegalGuide AI – Women's Legal Rights Assistant

## 1. Introduction

HerLegalGuide AI is an AI-powered Women's Legal Rights Assistant designed to provide information about women's legal rights and laws in India. It allows users to ask legal questions through a Telegram bot and receive easy-to-understand responses.

The system uses an AI model powered by Groq and a custom legal dataset stored in Google Drive. The n8n workflow connects these services and processes user questions to generate relevant answers.

## 2. Problem Statement

Many people are unaware of women's legal rights and the laws available to protect them. Finding reliable legal information can also be difficult.

HerLegalGuide AI aims to make basic legal information easier to access through a simple Telegram-based assistant.

## 3. Objectives

1. To provide information about women's legal rights in India.
2. To make legal information easier to understand.
3. To allow users to ask questions through Telegram.
4. To use a custom dataset as the source of legal information.
5. To generate relevant responses using an AI model.

## 4. Technologies Used

- **n8n:** Workflow automation and integration.
- **Telegram Bot API:** User interaction through a Telegram bot.
- **Groq AI:** AI chat model for generating responses.
- **JavaScript:** Dataset processing and question matching.
- **CSV/Excel:** Storage of legal information.
- **Google Drive:** Storage and access to the dataset.

## 5. System Architecture

The project follows this workflow:

```text
User
  |
  v
Telegram Bot
  |
  v
Telegram Trigger (n8n)
  |
  v
Download Dataset
  |
  v
Extract from File
  |
  v
Code in JavaScript
  |
  v
AI Agent + Groq Chat Model
  |
  v
Send Response to Telegram
  |
  v
User Receives Answer
```

## 6. How the System Works

1. The user sends a legal question through Telegram.
2. The Telegram Trigger receives the message in n8n.
3. The workflow downloads the dataset from Google Drive.
4. The Extract from File node reads the dataset.
5. The JavaScript node processes the dataset and identifies relevant information.
6. The AI Agent uses the retrieved information and Groq Chat Model to prepare a response.
7. The response is sent back to the user through Telegram.

## 7. Dataset Description

The project uses a custom CSV dataset containing information related to women's legal rights and laws in India.

The dataset provides legal information that the workflow uses to identify relevant content for user questions.

**Note:** The assistant's answers depend on the information available in the dataset. If relevant information is not available, the system may not provide a specific answer.

## 8. Main Features

- Telegram-based interaction.
- AI-generated responses.
- Custom legal dataset integration.
- Automated workflow using n8n.
- JavaScript-based dataset processing.
- Easy-to-understand legal information.

## 9. Implementation Files

| File/Folder | Description |
|---|---|
| `workflow.json` | Exported n8n workflow |
| `Javascript.js | JavaScript code used in the Code node |
| Dataset CSV | Custom women's legal rights dataset |
| `Screenshots/` | Project workflow and output screenshots |
| `README.md` | Project documentation |

## 10. Advantages

- Easy to access through Telegram.
- Saves time when searching for basic legal information.
- Uses a custom dataset.
- Automates question processing and response generation.
- Presents information in simple language.

## 11. Limitations

- Responses depend on the available dataset.
- The assistant may not answer questions outside its dataset.
- AI-generated responses may require verification.
- It does not replace advice from a qualified lawyer.

## 12. Future Enhancements

- Add more Indian laws and legal topics to the dataset.
- Support multiple Indian languages.
- Improve dataset search and matching.
- Add official government legal resources.
- Improve response accuracy and source references.

## 13. Conclusion

HerLegalGuide AI provides a simple way to access information about women's legal rights in India through a Telegram bot. By combining n8n automation, JavaScript, a custom dataset and Groq AI, the project demonstrates how AI can help make legal information more accessible and understandable.

## 14. Disclaimer

This project is developed for educational purposes. It provides general legal information and does not replace professional legal advice. Users should verify important legal matters through official sources or qualified legal professionals.

## 15. References

- [n8n Documentation](https://docs.n8n.io/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Groq Documentation](https://console.groq.com/docs)
- [India Code – Official Indian Laws](https://www.indiacode.nic.in/)
