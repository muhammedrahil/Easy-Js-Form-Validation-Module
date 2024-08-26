# Js-Form-Validation
js module for form validation

```javascript
class ErrorMessageDisplay {
    validateForm(errorMessages) {
        this.clearErrorMessages();
        let is_error = false;
        for (const errorMessageObject of errorMessages) {
            const { fieldId, errorMessage, type, maxlength = 256, minlength = 0, is_required = true, styles = "", classes = "", direct_style = false } = errorMessageObject;
            const field = document.getElementById(fieldId).value.trim();
            // const checkBox_field = document.getElementById(fieldId);
            switch (type) {
                case "email":
                    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
                    if (is_required) {
                        if (field.length == 0) {
                            this.displayErrorMessages(fieldId, errorMessage, styles, classes, direct_style);
                            is_error = true;
                        } else if (!emailRegex.test(field)) {
                            this.displayErrorMessages(fieldId, "Invalid Email Format", styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length > maxlength) {
                            this.displayErrorMessages(fieldId, `Email exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length < minlength) {
                            this.displayErrorMessages(fieldId, `Email is below minimum length of ${minlength}`, styles, classes, direct_style);
                            is_error = true;
                        }
                    } else {
                        if (field.length > 0) {
                            if (!emailRegex.test(field)) {
                                this.displayErrorMessages(fieldId, "Invalid Email Format", styles, classes, direct_style);
                                is_error = true;
                            } else if (field.length > maxlength) {
                                this.displayErrorMessages(fieldId, `Email exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                                is_error = true;
                            } else if (field.length < minlength) {
                                this.displayErrorMessages(fieldId, `Email is below minimum length of ${minlength}`, styles, classes, direct_style);
                                is_error = true;
                            }
                        }
                    }
                    break;
                case "phone_number":
                    const phoneNoRegex = /^\+?[0-9]+$/;
                    if (is_required) {
                        if (field.length == 0) {
                            this.displayErrorMessages(fieldId, errorMessage, styles, classes, direct_style);
                            is_error = true;
                        } else if (phoneNoRegex.test(field) == false) {
                            this.displayErrorMessages(fieldId, "Invalid Phone Number Format", styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length > maxlength) {
                            this.displayErrorMessages(fieldId, `Phone Number exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length < minlength) {
                            this.displayErrorMessages(fieldId, `Phone Number is below minimum length of ${minlength}`, styles, classes, direct_style);
                            is_error = true;
                        }
                    } else {
                        if (field.length > 0) {
                            if (phoneNoRegex.test(field) == false) {
                                this.displayErrorMessages(fieldId, "Invalid Phone Number Format", styles, classes, direct_style);
                                is_error = true;
                            } else if (field.length > maxlength) {
                                this.displayErrorMessages(fieldId, `Phone Number exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                                is_error = true;
                            } else if (field.length < minlength) {
                                this.displayErrorMessages(fieldId, `Phone Number is below minimum length of ${minlength}`, styles, classes, direct_style);
                                is_error = true;
                            }
                        }
                    }
                    break;
                case "general":
                    if (is_required) {
                        if (field.length == 0) {
                            this.displayErrorMessages(fieldId, errorMessage, styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length > maxlength) {
                            this.displayErrorMessages(fieldId, `Field exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                            is_error = true;
                        } else if (field.length < minlength) {
                            this.displayErrorMessages(fieldId, `Field is below minimum length of ${minlength}`, styles, classes, direct_style);
                            is_error = true;
                        }
                    } else {
                        if (field.length > 0) {
                            if (field.length > maxlength) {
                                this.displayErrorMessages(fieldId, `Field exceeds maximum length of ${maxlength}`, styles, classes, direct_style);
                                is_error = true;
                            } else if (field.length < minlength) {
                                this.displayErrorMessages(fieldId, `Field is below minimum length of ${minlength}`, styles, classes, direct_style);
                                is_error = true;
                            }
                        }
                    }
                    break;
                default:
                    break;
            }
        }
        return !is_error;
    }

    clearErrorMessages() {
        const errorMessages = document.querySelectorAll('.text-danger');
        errorMessages.forEach(errorMessage => errorMessage.parentNode.removeChild(errorMessage));
    }

    displayErrorMessages(fieldId, errorMessage, styles = "", classes = "", direct_style = false) {
        const errorContainer = document.createElement('div');
        errorContainer.className = 'text-danger '.concat(classes);
        if (!direct_style) {
            errorContainer.style = 'position:absolute;font-size:13px;'.concat(styles);
        } else {
            errorContainer.style = styles
        }
        errorContainer.innerHTML = errorMessage;
        const field = document.getElementById(fieldId);
        field.parentNode.appendChild(errorContainer);
    }
}

```


### Usage

```javascript
$('#{{form_id}}').on('submit', async function (event) {
    event.preventDefault();
    const errorMessageDisplay = new ErrorMessageDisplay();
    const errorMessages = [
        {
            "fieldId":"field_id",
            "errorMessage": "Field is required",
            "type": "general",
            "direct_style": true, // if you want style directly
            "styles": "font-size:13px;", // this is style part
            "classes": "float-start" // you can style here 
        },
        {
            "fieldId":"add-store-issue-packing-control",
            "errorMessage": "Field is required",
            "type": "general", // email and phone number
        }
    ];
    const isFormValid = errorMessageDisplay.validateForm(errorMessages);
    if (isFormValid){
    // Write your code
  }
});

```
