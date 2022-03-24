# Use of IntItelInput package

## Html file

    <html >
      <head>
       ...
        <link rel="stylesheet" href="../css/intlTelInput.css"  />
      </head>
      <body>
         ...
         <input type="phone" id="phone" >
         <div class='error'>
            @if($errors->has('telephone'))
            <small>{{ $errors->get('telephone')[0] }}</small>
            @endif
         </div>

         <script src="'.../js/intlTelInput.js"></script>
       </body>
    </html>
  
  
  ## JS file
  
     var input = document.querySelector("#phone");

    // Validate mobile number
    var input = document.querySelector("#phone"),
      errorMsg = document.querySelector("#error-msg");

    // here, the index maps to the error code returned from getValidationError - see readme
    var errorMap = ["Numéro invalide", "Code pays invalide", "Numéro trop court", "Numéro trop long", "Numéro invalide"];

    var iti = intlTelInput(input, {
        initialCountry: "CI",
        separateDialCode: true,
        dropdownContainer: document.body,
        placeholderNumberType: "MOBILE",
        geoIpLookup: function(callback) {
            $.get("http://ipinfo.io", function() {}, "jsonp").always(function(resp) {
                var countryCod = resp
                console.log(countryCod)
                callback(countryCod);
            });
        },
        utilsScript: "{{ asset('intlTelInput/js/utils.js') }}"
    });


    var reset = function() {
      input.classList.remove("error");
      errorMsg.innerHTML = "";
      errorMsg.classList.add("hide");
    };

    // on blur: validate
    input.addEventListener('blur', function() {
      reset();
      if (input.value.trim()) {

        if (!iti.isValidNumber()) {
          input.classList.add("error");
          var errorCode = iti.getValidationError();
          errorMsg.innerHTML = errorMap[errorCode];
          errorMsg.classList.remove("hide");
        }
      }
    });

    // on keyup / change flag: reset
    input.addEventListener('change', reset);
    input.addEventListener('keyup', reset);

    function loadContry(){
      var country_code = document.querySelector("#country");
      console.log(country_code);
      this.phoneInput = intlTelInput(input, {
      initialCountry: "CI",
      separateDialCode: true,
      dropdownContainer: document.body,
      placeholderNumberType: "MOBILE",
      geoIpLookup: function(callback) {
          $.get("http://ipinfo.io", function() {}, "jsonp").always(function(resp) {
              var countryCod = resp
              console.log(countryCod)
              callback(countryCod);
          });
      },
      utilsScript: ".../js/utils.js"
      });
    };
  
  ## Dial code
  
 To get dial code add this ligne on download file intITelInput.js file after line 1990:
      
     // intITelInput.js file
     
     .... 
     1990 this.selectedFlag.setAttribute("title", title);
     1991
     1992  //Dial code autocomplet
     1993  document.getElementById('dial_code').value= this.selectedCountryData.dialCode; // Add line
  
