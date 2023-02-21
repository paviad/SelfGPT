# SelfGPT

This is the documentation for the SelfGPT WhatsApp bot. 

This bot allows you to contact GPT3 directly from WhatsApp.
This bot also allows you to save your own personal data and later search and retrieve it using GPT3 to generate a response. In the `examples` folder you can see several examples of how to use this bot so you don't have to remember anything ever again.

**Note:** While the code in its current state is very useful I would highly appreciate any contributions that would make it more "production-ready".

## Current Features

The bot can be used in two ways:
- **No-Command**: This is the default mode. The bot will respond to any message you send it through the GPT3 regular completion API.
- **Commands**: This mode is activated by sending the bot a message starting with the command character. The default command character is `/`. 
  - Currently supported commands:
    - **`/h`**: Show a help message that lists all the commands
    - **`/s <message>`**: Save the message to the database
    - **`/q <question>`**: Ask a question about the database and get a response from GPT3
    - **`/f <message>`**: Find related messages in the database

## Future Features

- [ ]  Add a way to delete messages from the database.
- [ ]  Add a way to edit messages in the database.
- [ ]  Receive PDF/Word files and parse them into the database.
- [ ]  Receive audio messages, transcribe them (using OpenAI Whisper or something similiar) and parse them into the database.
- [ ]  Add ability to upload photos, embedd them with OpenAI CLIP and enable searching them with natural language.

## How to use selfGPT
1. Clone or download the code from the repository.

2. Set an account on [OpenAI](https://beta.openai.com/) and get your API key.

3. Set up a **Twilio** account:
   - Set an account on [Twilio](https://www.twilio.com/). 
   - Go to Twilio's [whatsapp website](https://www.twilio.com/whatsapp) and sign up.
   - Connect Twilio with WhatsApp (see [here](https://www.pragnakalp.com/create-whatsapp-bot-with-twilio-using-python-tutorial-with-examples/) for a tutorial).
   - Save the contact details given by Twilio.
   - Send message to the number given by Twilio as instructed in the tutorial.

4. If you want to deploy to *Azure* skip to [Deploy to Microsoft Azure](#deploy-to-microsoft-azure).

5. Copy the files `user/config/config.yaml.example` and `user/config/ngrok.yml.example` to the same location but without the `.example` extension. Those files (without the `.example` extension) are ignored by git and you may safely put your secrets in them.

6. Add the API key to the `user\config\config.yaml` file.

7. If you are using _Docker_ skip to the [Running SelfGPT using Docker](#running-selfgpt-using-docker)

8. Install the requirements using `pip install -r requirements/requirements.txt` (you might need to run it with the `--user` flag depending on your setup)

9. The database will be saved into the `user\data` folder.

10.  **NGROK**
    - Download and install [NGROK](https://ngrok.com/download).
    - Make sure you configure NGROK with your auth token (visit https://dashboard.ngrok.com/get-started/your-authtoken)
    - Open the terminal and run `ngrok http 5000` (5000 is the default port used by Flask).
    - From that Ngrok links, copy the HTTPS link URL
    - Go to back to Twilio's sand box for whatsapp and add the URL given by NGROK with the suffix */wasms*  to the box marked `WHEN A MESSAGE COMES IN` (see [here](https://www.pragnakalp.com/create-whatsapp-bot-with-twilio-using-python-tutorial-with-examples/) for a tutorial) and press save.
  
That's it! You can now use the bot by running `selfgpt.py`. Notice that tha bot runs only as long as the program runs.

## Running SelfGPT using Docker

1. Add your NGROK auth token to `user/config/ngrok.yml` (`yml` not `yaml`)
   (visit https://dashboard.ngrok.com/get-started/your-authtoken)

2. You need to set up your _Twilio_ account similar to step 8 in the previous section.

3. Run `build-docker.cmd`

4. Run `run-docker.cmd`

5. Finally, copy the NGROK forwarding address into your Twilio configuration.

## Deploy to Microsoft Azure

1. Create an account at Microsoft Azure if you don't have any [here](https://signup.live.com/?lic=1), notice that you'll need a credit card for this (you do not have to pay anything).

2. Install the *Azure CLI* tools, instructions [here](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) and verify they work properly by running `az login` and then `az account list` and making sure it completes without error and outputs the correct information.

2. Enter the `azure` folder in the cloned repo, and run `azure-deploy.cmd` - it will ask you a few questions, and then deploy everything to *Azure* - it's as simple as that! **Note:** remember or copy the app name that you have given, you will need it in the next step. 

3. Once the app is deployed, you can enter its address in the _Twilio_ incoming message hook (remember to suffix it with `/wasms`). The address is `https://<<app name>>.azurewebsites.net/wasms`. Reminder: the incoming message hook can be found in the Twilio console under Messaging -> Settings -> WhatsApp sendbox settings

**NOTE**: If you are using a free app service plan (as is the default in the repo at the moment), note that it might take a while for the app to "warm up" so be patient (might take up to 3 minutes, or maybe more). The way to deal with that is to send a `/h` command to the bot and if it doesn't respond, wait 3 minutes and send another `/h` - only if it responds then you may send other commands to it.

## Known issues

- The Twilio account disconnects every 72 hours: Reconnecting is easy. Just send the same message you sent on step 8 that connected you in the first place.
- The Ngrok changes URL: Sometime the bot would stop responding. This can be caused by the Ngrok changing the URL. In this case you have to repeat step 9.

## How to contribute

This is an open source project and contributions are welcome. If you want to contribute, you can do so by forking the repository and making a pull request. If you have any questions or suggestions, you can open an issue or contact me directly.

## License

This project is licensed under the AGPL-3.0 License - see the [LICENSE](LICENSE) file for details

## Acknowledgments

Huge credit for Pragnakalp for making this great tutorial on how to create a WhatsApp bot using Twilio and Flask. I used his tutorial as a base for this project. You can find the tutorial [here](https://www.pragnakalp.com/create-whatsapp-bot-with-twilio-using-python-tutorial-with-examples/).
