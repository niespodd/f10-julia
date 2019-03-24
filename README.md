```
const Alexa = require('ask-sdk-core');

const PortfolioIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.intent.name === 'PortfolioIntentHandler';
    },
    handle(handlerInput) {
        const speechText = `Good morning Romeo. The latest value of your portfolio is 1.55CHF Million. Its value has increased by 
        200 thousand Swiss Francs since the last valuation, thanks to the positive performance of your pharmaceutical stocks. Well 
        done. However, the portfolio exceeded the asset allocation limit in the sector. Would you like me to reduce your pharmaceutical 
        asset allocation back to the 5 per cent, or only reduce the pharmaceutical stocks? Or, do you want to talk to your relationship manager?`;
        
        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt('Huh?')
            .getResponse();
    }
};

const CallScheduleIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.intent.name === "CallScheduleIntentHandler";
    },
    handle(handlerInput) {
        const speechText = `Great. Your relationship manager has a free slot in 30 minutes and will call you then. Ok?`;
        
        return handlerInput.responseBuilder
            .speak(speechText)
            .getResponse();
    }
}

const CallScheduleConfirmIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.intent.name === "AMAZON.YesIntent";
    },
    handle(handlerInput) {
        const speechText = `Done.`;
        
        return handlerInput.responseBuilder
            .speak(speechText)
            .getResponse();
    }
}


const HelpIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && handlerInput.requestEnvelope.request.intent.name === 'AMAZON.HelpIntent';
    },
    handle(handlerInput) {
        const speechText = 'You can say hello to me! How can I help?';

        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};
const CancelAndStopIntentHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'IntentRequest'
            && (handlerInput.requestEnvelope.request.intent.name === 'AMAZON.CancelIntent'
                || handlerInput.requestEnvelope.request.intent.name === 'AMAZON.StopIntent');
    },
    handle(handlerInput) {
        const speechText = 'Goodbye!';
        return handlerInput.responseBuilder
            .speak(speechText)
            .getResponse();
    }
};
const SessionEndedRequestHandler = {
    canHandle(handlerInput) {
        return handlerInput.requestEnvelope.request.type === 'SessionEndedRequest';
    },
    handle(handlerInput) {
        // Any cleanup logic goes here.
        return handlerInput.responseBuilder.getResponse();
    }
};

// Generic error handling to capture any syntax or routing errors. If you receive an error
// stating the request handler chain is not found, you have not implemented a handler for
// the intent being invoked or included it in the skill builder below.
const ErrorHandler = {
    canHandle() {
        return true;
    },
    handle(handlerInput, error) {
        console.log(`~~~~ Error handled: ${error.message}`);
        const speechText = `Sorry, I couldn't understand what you said. Please try again.`;

        return handlerInput.responseBuilder
            .speak(speechText)
            .reprompt(speechText)
            .getResponse();
    }
};

// This handler acts as the entry point for your skill, routing all request and response
// payloads to the handlers above. Make sure any new handlers or interceptors you've
// defined are included below. The order matters - they're processed top to bottom.
exports.handler = Alexa.SkillBuilders.custom()
    .addRequestHandlers(
        PortfolioIntentHandler,
        CallScheduleConfirmIntentHandler,
        CallScheduleIntentHandler,
        HelpIntentHandler,
        CancelAndStopIntentHandler,
        SessionEndedRequestHandler
        ) // make sure IntentReflectorHandler is last so it doesn't override your custom intent handlers
    .addErrorHandlers(
        ErrorHandler)
    .lambda();
```
