# Serverless form submission via AWS Lambda to Slack channel

## Preparations

Before deploying serverless backend for a web from you need 

1. Google reCAPTCHA<br>
Register yoursite for [Google reCAPTCHA](https://www.google.com/recaptcha/) and get your site key and secret key.

2. Slack webhook for posting the form to a Slack channel<br>
Register [Slack](https://slack.com/) workspace and [create an incoming webhook for posting to a channel](https://slack.com/intl/en-fi/help/articles/115005265063-Incoming-WebHooks-for-Slack)
3. (optional) Route53 zone for a custom name for the backend
4. (optional) ACM certificate at us-west-1 to match your custom name

## Setup

Once you have all above preparations done

1. Build Cloudformation stack with feedbackapi.yaml -template<br>
Template will take 5 parameters
- <code>APIStage</code> is just a name for your API stage. You can leave it as default "prod" or call it whatever you want.
- <code>ReCaptchaSecret</code> is the SECRET key you got when registered your site to Google reCAPTCHA.
- <code>Webhook</code> is URL of webhook for posting to Slack channel.
- <code>CustomDomain</code> is optional parameter. If you have Route53 domain you can register a custom name for the API Gateway backend. Full name of your API is going to STACKNAME.CustomDomain, e.g. feedback.example.com
- <code>CustomCert</code> is optional parameter. If you supplied CustomDomain this must be an ARN of matching ACM certificate at __us-east-1__ -region. Stack itself can be built into any region you choose.
<br><br>Once stack is ready, you will find in outputs the URL of API.

2. Modify feedback.html -sample form<br>
Replace ```__reCaptchaSitekey__``` and 
```https://URL/TO/YOUR/AWS/APIGW/``` with real values you obtained earlier.
```javascript
          <button type="submit" class="g-recaptcha form__button"
		    data-sitekey="__reCaptchaSitekey__"
            data-callback="handleFormSubmit">Submit</button>
        </form>
      </td>
    </tr>
  </table>

  <script src='https://www.google.com/recaptcha/api.js' async defer></script>
  <script>
    var handleFormSubmit = function () {
      var formApiEndpoint = "https://URL/TO/YOUR/AWS/APIGW/"
      var successEl = document.querySelector('.form__success')
```

3. Open feedback.html in your browser and submit the form to your Slack channel!