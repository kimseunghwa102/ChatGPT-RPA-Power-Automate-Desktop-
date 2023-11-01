# ChatGPT+RPA(Power Automate Desktop)
Power Automate Desktop에서 ChatGPT의 The chat completion object를 사용하는 방법입니다. 
Power Automate Desktop의 프로세스를 내보낼 때 사용되는 확장자는 .pad
주석 처리는 // 을 사용합니다. 

## ChatGPT에게 명령할 프롬프르를 INPUT으로 작성합니다.
Display.InputDialog Title: $'''CahtGPT''' Message: $'''질문을 주세요! ''' InputType: Display.InputType.SingleLine IsTopMost: False UserInput=> UserInput

##  Prompt에 INPUT값을 Post방식으로 요청합니다. (Autorization에는 ChatGPT API KEY값을 입력합니다.)
Web.InvokeWebService.InvokeWebService Url: $'''https://api.openai.com/v1/completions''' Method: Web.Method.Post Accept: $'''application/json''' ContentType: $'''application/json''' CustomHeaders: $'''Authorization: ''' RequestBody: $'''{
    \"model\": \"gpt-3.5-turbo-instruct\",
    \"prompt\": \"%UserInput%\",
    \"max_tokens\": 500,
    \"temperature\": 0
  }''' ConnectionTimeout: 30 FollowRedirection: True ClearCookies: False FailOnErrorStatus: False EncodeRequestBody: False UserAgent: $'''Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.8.1.21) Gecko/20100312 Firefox/3.6''' Encoding: Web.Encoding.AutoDetect AcceptUntrustedCertificates: False ResponseHeaders=> WebServiceResponseHeaders Response=> WebServiceResponse StatusCode=> StatusCode

##  Json형식으로 값을 변환해 줍니다.  
Variables.ConvertJsonToCustomObject Json: WebServiceResponse CustomObject=> JsonAsCustomObject

##  OUTPUT값을 메시지창에 나타냅니다.
Display.ShowMessageDialog.ShowMessage Message: JsonAsCustomObject['choices'][0]['text'] Icon: Display.Icon.None Buttons: Display.Buttons.OK DefaultButton: Display.DefaultButton.Button1 IsTopMost: False ButtonPressed=> ButtonPressed2
