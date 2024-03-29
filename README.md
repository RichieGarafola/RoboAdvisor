# The Power of the Cloud and Unsupervised Learning

# Robo Advisor for Retirement Plans

![Robot](Images/robot.jpg)

_Photo by [Alex Knight](https://www.pexels.com/@alex-knight-1272316?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/high-angle-photo-of-robot-2599244/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) | [Free License](https://www.pexels.com/photo-license/)_

### Background

You were hired as a digital transformation consultant by one of the most prominent retirement plan providers in the country; they want to increase their client portfolio, especially by engaging young people. Since machine learning and NLP are disrupting finance to improve customer experience, you decide to create a robo advisor that could be used by customers or potential new customers to get investment portfolio recommendations for retirement.

In this homework assignment, you will combine your new Amazon Web Services skills with your already mastered Python superpowers, to create a bot that will recommend an investment portfolio for a retirement plan.

You are asked to accomplish the following main tasks:

1. **[Initial Robo Advisor Configuration:](#Initial-Robo-Advisor-Configuration)** Define an Amazon Lex bot with a single intent that establishes a conversation about the requirements to suggest an investment portfolio for retirement.

2. **[Build and Test the Robo Advisor](#Build-and-Test-the-Robo-Advisor):** Make sure that your bot is working and responding accurately along with the conversation with the user, by building and testing it.

3. **[Enhance the Robo Advisor with an Amazon Lambda Function:](#Enhance-the-Robo-Advisor-with-an-Amazon-Lambda-Function)** Create an Amazon Lambda function that validates the user's input and returns the investment portfolio recommendation. This task includes testing the Amazon Lambda function and making the integration with the bot.

---

### Files

- [lambda_function.py](Starter_Files/lambda_function.py)
- [correct_dialog.txt](Test_Cases/correct_dialog.txt)
- [age_error.txt](Test_Cases/age_error.txt)
- [incorrect_amount_error.txt](Test_Cases/incorrect_amount_error.txt)
- [negative_age_error.txt](Test_Cases/negative_age_error.txt)

---

### Instructions

#### Initial Robo Advisor Configuration

In this section, you will create the `RoboAdvisor` bot and add an intent with its corresponding slots.

Sign in into your AWS Management Console and [create a new custom Amazon Lex bot](https://console.aws.amazon.com/lex/home). Use the following parameters:

- **Bot name:** RoboAdvisor
- **Output voice**: Salli
- **Session timeout:** 5 minutes
- **Sentiment analysis:** No
- **COPPA**: No
- **Advanced options**: No
- _Leave default values for all other options._

Create the `RecommendPortfolio` intent, and configure some sample utterances as follows (you can add more utterances as you wish):

- I want to save money for my retirement
- I'm ​`{age}​` and I would like to invest for my retirement
- I'm `​{age}​` and I want to invest for my retirement
- I want the best option to invest for my retirement
- I'm worried about my retirement
- I want to invest for my retirement
- I would like to invest for my retirement

This bot will use four slots, three using built-in types and one custom slot named `riskLevel`. Define the three initial slots as follows:

| Name             | Slot Type            | Prompt                                                                 |
| ---------------- | -------------------- | ---------------------------------------------------------------------- |
| firstName        | AMAZON.US_FIRST_NAME | Thank you for trusting me to help, could you please give me your name? |
| age              | AMAZON.NUMBER        | How old are you?                                                       |
| investmentAmount | AMAZON.NUMBER        | How much do you want to invest?                                        |

The `riskLevel` custom slot will be used to retrieve the risk level the user is willing to take on the investment portfolio. Create this custom slot as follows:

- Select the `+` icon next to 'Slot Types' in the 'Editor' on the left side of the screen.
- Choose `create custom slot` from the resulting display window.
- For **Slot type name**, type: riskLevel
- Select the radial dial button next to **Restrict to Slot values and synonyms**, then fill in the appropriate values and synonums. _Example_: Low, Minimal; High, Maximum.
- Click `Add slot to intent` when finished.

To format the response cards for the intent, click on the gear icon next to the intent as seen in the image below:

![gear_icon](Images/gear_icon.png)

Next, input the following data in the resulting display window:

- **Prompt:** What level of investment risk would you like to take?
- **Maximum number of retries:** 2
- **Prompt response cards:** 4

Configure the response cards for the `riskLevel` slot as is shown bellow:

| Card 1                             | Card 2                             |
| ---------------------------------- | ---------------------------------- |
| ![Card 1 sample](Images/card1.png) | ![Card 2 sample](Images/card2.png) |

| Card 3                             | Card 4                             |
| ---------------------------------- | ---------------------------------- |
| ![Card 3 sample](Images/card3.png) | ![Card 4 sample](Images/card4.png) |

**Note:** You can download free icons from [this website](https://www.iconfinder.com/) or you can use the icons provided in the [`Icons` directory](Icons/).

Move to the _Confirmation Prompt_ section, and set the following messages:

- **Confirm:** Would you like me to search for the best investment portfolio for you now?
- **Cancel:** I will be pleased to assist you in the future.

Leave the error handling configuration for the `RecommendPortfolio` bot with the default values.

![Error handling configuration](Images/error_handling.png)

#### Build and Test the Robo Advisor

In this section, you will test your Robo Advisor. To build your bot, click on the `Build` button in the upper right hand corner. Once the build is complete, test it in the chatbot window. You should see a conversation like the one below.

![Robo Advisor test](Images/bot-test-no-lambda.gif)

#### Enhance the Robo Advisor with an Amazon Lambda Function

In this section, you will create an Amazon Lambda function that will validate the data provided by the user on the Robo Advisor. Start by creating a new lambda function from scratch and name it `recommendPortfolio`. Select Python 3.7 as runtime.

In the Lambda function, start by deleting the AWS generated default lines of code, then paste in the starter code provided in [lambda_function.py](Starter_Files/lambda_function.py) and complete the `recommend_portfolio()` function by following these guidelines:

##### User Input Validation

- The `age` should be greater than zero and less than 65.
- the `investment_amount` should be equal to or greater than 5000.

##### Investment Portfolio Recommendation

Once the intent is fulfilled, the bot should response with an investment recommendation based on the selected risk level as follows:

- **none:** "100% bonds (AGG), 0% equities (SPY)"
- **very low:** "80% bonds (AGG), 20% equities (SPY)"
- **low:** "60% bonds (AGG), 40% equities (SPY)"
- **medium:** "40% bonds (AGG), 60% equities (SPY)"
- **high:** "20% bonds (AGG), 80% equities (SPY)"
- **very high:** "0% bonds (AGG), 100% equities (SPY)"

Be creative while coding your solution, you can have all the code on the `recommend_portfolio()` function, or you can split the functionality across different functions, put your Python coding skills in action!

Once you finish coding your lambda function, test it using the [sample test cases](Test_Cases/) provided for this homework.

After successfully testing your code, open the Amazon Lex Console and navigate to the `RecommendPortfolio` bot configuration, integrate your new lambda function by selecting it in the _Lambda initialization and validation_ and _Fulfillment_ sections. Build your bot, and you should have a conversation as follows.

![Robo Advisor test with Lambda](Images/bot-test-with-lambda.gif)

### Submission

You should create a brand new repository in GitHub and upload the following files to your repo.

- A python script with your final lambda function.

- From the Amazon Lex Console, export your bot, intent, and slot using `Amazon Lex` as the target platform, and upload the ZIP files to your repo.

- Create a short video or animated GIF showing a demo of your Robo Advisor in action from the test window. Upload the video or animated GIF file to your repo.

Once you have uploaded all the files into the repo, post a link to your homework's repository in BootCamp Spot.

### Hints

- Make sure your intent and slot names are named correctly in your Lambda code. The names in Lex should match the names in Lambda exactly:

![Lex_Names1](Images/Lex_names1.png)
![Lex_Names2](Images/Lex_names2.png)

- You may have to refresh the Lex intent page after creating the custom slot and the lambda function in order to see them in the options.

- If you are using a Mac, you can create a screen-recording using the built-in QuickTime player. Follow [this link](https://support.apple.com/en-us/HT208721#quicktime) to learn more.

- If you are using Windows 10, you can create a screen-recording using the built-in Xbox Game Bar. Follow [this link](https://beta.support.xbox.com/help/friends-social-activity/share-socialize/record-game-clips-game-bar-windows-10) to learn more.

---



### Requirements

<details>
<summary>Robo Advisor for Retirement Plans</summary>

#### Initial RoboAdvisor Configuration (35 points)

##### To receive all points, your code must:

- Create a RoboAdvisor with the requested parameters. (10 points)
- Create the RecommendPortfolio intent and configure it with the proper name utterances. (10 points)
- Create the RiskLevel custom slot with proper card slots. (10 points)
- Build and test the RoboAdvisor with the default error handling configuration. (5 points)

#### Enhance RoboAdvisor with Amazon Lambda Function (35 points)

##### To receive all points, your code must:

- Validate the user's input. (9 points)
- Provide an "Investment Portfolio Recommendation" based on the user's selected risk values. (9 points)
- Test your Lambda Function with the provided sample test cases. (8 points)
- Integrate your Lambda Function with the RoboAdvisor. (9 points)

#### Coding Conventions and Formatting (10 points)

##### To receive all points, your code must:

- Place imports at the beginning of the file, just after any module comments and docstrings and before module globals and constants. (3 points)
- Name functions and variables with lowercase characters and with words separated by underscores. (2 points)
- Follow Don't Repeat Yourself (DRY) principles by creating maintainable and reusable code. (3 points)
- Use concise logic and creative engineering where possible. (2 points)

#### Deployment and Submission (10 points)

##### To receive all points, you must:

- Submit a link to a GitHub repository that’s cloned to your local machine and contains your files. (5 points)
- Include appropriate commit messages in your files. (5 points)

#### Code Comments (10 points)

##### To receive all points, your code must:

- Be well commented with concise, relevant notes that other developers can understand. (10 points)

</details>

<details>
