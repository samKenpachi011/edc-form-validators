[![Build Status](https://app.travis-ci.com/samKenpachi011/edc-form-validators.svg?branch=develop)](https://app.travis-ci.com/samKenpachi011/edc-form-validators)

[![Coverage Status](https://coveralls.io/repos/github/EDC-Upgrade/edc-form-validators/badge.svg?branch=develop)](https://coveralls.io/github/EDC-Upgrade/edc-form-validators?branch=develop)

[![Language](https://img.shields.io/badge/Language-Python-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/Version-1.0.0-blue.svg)](https://github.com/samKenpachi011/edc-form-validators/releases/tag/v1.0.0)
[![Log Scan Status](https://img.shields.io/badge/Log%20Scan-Passing-brightgreen.svg)](https://app.travis-ci.com/github/samKenpachi011/edc-form-validators/logscans)

# edc-form-validators

Form validator classes for ModelForms


### ModelForm `FormValidator`

`FormValidator` simplifies common patterns used in `ModelForm.clean`. For example, if there is a response to field A then there should not a be response to B and visa-versa.

Declare a form with it's `form_validator` class and use `FormValidatorMixin`:

        class MyFormValidator(FormValidator):

            def clean(self):
                self.required_if(
                    YES,
                    field='f1',
                    field_required='f2')
                ...

        class MyModelForm(FormValidatorMixin, forms.ModelForm):

            form_validator_cls = MyFormValidator

            class Meta:
                model = TestModel
                fields = '__all__'


#### Testing:

Test the `form_validator` without having to instantiate the `ModelForm`:

    def test_my_form_validator(self):
        options = {
            'f1': YES,
            'f2': None}
        form_validator = MyFormValidator(cleaned_data=options)
        self.assertRaises(ValidationError, form_validator.validate)
        self.assertIn('f2', form_validator._errors)
