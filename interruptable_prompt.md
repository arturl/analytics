# Bot Framework 4.1 Interruptable Prompt


## Summary
Cafe Bot and Enterprise Bot  independently identified handling interruptions as an ability bots should handle.  They both solved it in a similar way using LUIS/QnA (Dispatch) to determine the customer response.  This proposes a new prompt that integrates LUIS/QnA models for assisting in interruptions and flexible data collection for model improvement, modem measurement and custom customer reports.

## Developer Stories
As a sdk user, I want to be able to handle help and cancel  in my prompts in order to provide a good customer experience.
As a sdk user, I want to be able to handle arbitrary chit chat in my prompts in order to provide a good customer experience.
As a sdk user, I want to be able to handle other classes of responses (aggressive behavior, questioning validity of prompt, etc) in my prompts in order to provide a good customer experience.
As a sdk user, I want to be able to integrate language models to assist in interpreting if the user is interrupting normal flow  in order to provide a good customer experience.
As a sdk user, I want reports or feedback in order to understand if my language models are working.
As a sdk user, I want the ability to the ability to  provide customer training data per prompt into the language understanding models  in order to increase accuracy. 
As a sdk user, I want a curating process to curate per prompt user training data into the language understanding models in order to reject faulty data.
As a sdk user, I want the ability to the language understanding models to update themselves in a timely manner so the customer experience improves with low latency.
As a analyst, I want the ability modify the way the data is stored in order to construct the correct queries for reports.

## Business Stories
As a analyst, I want to be able to create dialog level data together easily in order to construct dialog-level reports.
As a analyst, I want to be able to understand where in dialog flow customers are not responding in order to understand my customer behavior and modify my prompts.
As a analyst, I want the ability to see raw customer data, in order to understand what customers are seeking within the bot.


```json
{
    "version": "0.1",
    // FUTURE  - Data Driven Dialog
    "name": "greetings",
        // /Data Driven Prompt
        "prompts" : [
            {
                "username_prompt" : {
                    "prompt" : "What is your name?",
                    "type" : "string",
                    // FUTURE - Pre/post hook scripts.
                    "prevalidator" : "<.csx/.js/.python reference>",
                    "postvalidator" : "<.csx/.js/.python> reference",
                    // runmode [Training | Dev | None]
                    "runmode" : "confirm",
                    // Dispatch/QnA/LUIS first class with prompt.
                    "models": [
                        {
                            "usernameDispatcher" : {
                                "type":"luis",
                                "description": "Common model",
                                "location" : "<model file>"
                                }
                        }
                    ],
                    // Data collection first class with prompt.
                    "data_capture" : [
                        {
                            "custom_event_name": "custom1",
                            "fields": ["activity.ActivityId", "luis.usernameDispatcher.Topintent", "luis.usernameDispatcher.Entities"]
                        },
                        { 
                            "custom_event_name": "top_intent_activity",
                            "fields": ["activity.ActivityId", "prompt.Value", "common.datetime"]
                        }
                    ]
                }
            }
        ],
        // FUTURE: Override all prompts
        "run_mode" : "confirm",
        // FUTURE: Data capture across all prompts
        "data_capture" : [
            {
                "custom_event_name": "across_prompt_data",
                "fields": ["username_prompt.luis.usernameDispatcher.Topintent", "username_prompt.luis.usernameDispatcher.Entities"]
            }
        ]
    }
}
```