# Easy-Js-Form-Validation

Js-Form-Validation is a lightweight JavaScript module designed to streamline and enhance form validation in your web applications. It provides a simple, yet powerful way to validate form fields such as emails, phone numbers, and general text inputs. This module offers customizable error message displays with options for adding custom styles and classes, making it easy to integrate into any project.


## Features

- Email Validation: Checks if the input matches a standard email format and allows you to set custom minimum and maximum length requirements.
  
- Phone Number Validation: Validates phone numbers against a regex pattern and provides customizable length restrictions.
  
- General Field Validation: Ensures that required fields are filled and within specified length constraints.
  
- Customizable Error Messages: Display error messages with custom styles, classes, and positioning.
  
- Direct Style Application: Apply styles directly to error messages or through CSS classes.
  
- Flexible Configuration: Supports both required and optional fields with different validation rules.
  
- Error Clearing: Automatically clears previous error messages before validating the form.
  
- Easy Integration: Simple to integrate with any form, just pass in the form ID and error message configurations.

- Event Handling: Trigger form validation on form submission, and prevent submission if validation fails.


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


## Usage

Here’s an example of how you can use Js-Form-Validation in your project:

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


In this example:

- Field Validation: You can define multiple fields to be validated, each with its own validation type and error message.

- Customization: Apply custom styles or CSS classes to error messages for better UI integration.

- Direct Style Application: You can choose to style error messages directly using the `direct_style` option.
