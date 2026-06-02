# Create and use fal.ai API keys

There are 2 types of API keys in fal.ai
- **API Keys:** For running models and calling inference endpoints. Cannot manage keys, billing, usage, or compute. Recommended for most use cases.
    - For our purposes, the API key allows Lightroom and the plugins to run AI models on your behalf
- **Admin Keys:** Full access — run models plus manage API keys, billing, usage data, and compute instances. Use for account management, automation, and integrations.
   - For our purposes, the plugins use this to show your your current balance

## Creating Keys
1. From the top menu in fal.ai, click **Settings** and then **API Keys**<br>
<img width="465" height="404" alt="image" src="https://github.com/user-attachments/assets/0cbba192-d61d-4660-b592-0c622b61382e" />

<br><br>

2. Click **+ Add key**<br>
<img width="670" height="245" alt="image" src="https://github.com/user-attachments/assets/b5a25b39-4534-4b81-a811-444963f7020c" />
<br><br>

### API Key
3. Make sure the **Scope** is set to **"API"**
- Put something in the description
- ***Before you click "Create Key", make sure you are ready to copy and save this key to a secure location***<br>
After it appears, it cannot be accessed directly again.
- If you do not copy and save it, just create a new key.  There is not a limit
- Click **Create Key**<br>
<img width="395" height="339" alt="Screenshot 2026-06-02 161354" src="https://github.com/user-attachments/assets/34770ffb-f54f-4ca0-9311-a70a61059513" />


4. Your new key will appear. My new key is redacted in the screenshot below.
- Copy your key and save it in a secure place.
- !! Anyone who gets your key can execute fal.ai processes on your behalf.<br>
<img width="385" height="416" alt="Screenshot 2026-06-02 161856" src="https://github.com/user-attachments/assets/fc4c30a9-389c-4308-bb2d-6f1280eb1ec2" />
<br><br>

5. You are now ready to paste this key into the plugins in Lightroom
<br><br>

### ADMIN Key (Optional)

If you want the plugins in Lightroom to show you current balance information from fal.ai, you will need an **Admin** key as well.  *It is not required.*
<br><br>

6. While you are stil on the **API Keys** page, create an Admin key as well.<br>
- Change the **Scope** and set to **"ADMIN"** <br>
   <img width="397" height="144" alt="image" src="https://github.com/user-attachments/assets/4c78c9cf-9751-4c37-91e2-f1129d029ed0" />
<br><br>

7. Follow the remainining steps of creating an API key and make sure to copy it and save it securely<br>
8. You can now add the Admin key to the plugins

