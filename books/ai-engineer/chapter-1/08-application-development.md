### Application development

With traditional ML engineering, where teams build applications using their proprietary models, the model quality is a differentiation. With foundation models, where many teams use the same model, differentiation must be gained through the application development process.

The application development layer consists of these responsibilities: evaluation, prompt engineering, and AI interface.

#### Evaluation

_Evaluation_ is about mitigating risks and uncovering opportunities. Evaluation is necessary throughout the whole model adaptation process. Evaluation is needed to select models, to benchmark progress, to determine whether an application is ready for deployment, and to detect issues and opportunities for improvement in production.

#### Prompt engineering and context construction

_Prompt engineering_ is about getting AI models to express the desirable behaviors from the input alone, without changing the model weights.  The Gemini evaluation story highlights the impact of prompt engineering on model performance. By using a different prompt engineering technique, Gemini Ultra’s performance on MMLU went from 83.7% to 90.04%.

It’s possible to get a model to do amazing things with just prompts. The right instructions can get a model to perform the task you want, in the format of your choice. Prompt engineering is not just about telling a model what to do. It’s also about giving the model the necessary context and tools to do a given task. For complex tasks with long context, you might also need to provide the model with a memory management system so that the model can keep track of its history.  [Chapter 5](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch05.html#ch05a_prompt_engineering_1730156991195551)  discusses prompt engineering, and  [Chapter 6](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch06.html#ch06_rag_and_agents_1730157386571386)  discusses context construction.

#### AI interface

_AI interface_ means creating an interface for end users to interact with your AI applications.  Before foundation models, only organizations with sufficient resources to develop AI models could develop AI applications. These applications were often embedded into the organizations’ existing products. For example, fraud detection was embedded into Stripe, Venmo, and PayPal. Recommender systems were part of social networks and media apps like Netflix, TikTok, and Spotify.

With foundation models, anyone can build AI applications. You can serve your AI applications as standalone products or embed them into other products, including products developed by other people. For example, ChatGPT and Perplexity are standalone products, whereas GitHub’s Copilot is commonly used as a plug-in in VSCode, and Grammarly is commonly used as a browser extension for Google Docs. Midjourney can either be used via its standalone web app or via its integration in Discord.

There need to be tools that provide interfaces for standalone AI applications or make it easy to integrate AI into existing products. Here are just some of the interfaces that are gaining popularity for AI applications:

-   Standalone web, desktop, and mobile apps.[26](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch01.html#id668)
    
-   Browser extensions that let users quickly query AI models while browsing.
    
-   Chatbots integrated into chat apps like Slack, Discord, WeChat, and WhatsApp.
    
-   Many products, including VSCode, Shopify, and Microsoft 365, provide APIs that let developers integrate AI into their products as plug-ins and add-ons. These APIs can also be used by AI agents to interact with the world, as discussed in [Chapter 6](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch06.html#ch06_rag_and_agents_1730157386571386).
    

While the chat interface is the most commonly used, AI interfaces can also be voice-based (such as with voice assistants) or embodied (such as in augmented and virtual reality).

These new AI interfaces also mean new ways to collect and extract user feedback. The conversation interface makes it so much easier for users to give feedback in natural language, but this feedback is harder to extract. User feedback design is discussed in  [Chapter 10](https://learning.oreilly.com/library/view/ai-engineering/9781098166298/ch10.html#ch10_ai_engineering_architecture_and_user_feedback_1730130985311851).
