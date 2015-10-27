# gravity-form-snippits
```php
add_filter( 'gform_field_validation_1_4', 'validate_phone_1_4', 10, 4 );
function validate_phone_1_4( $result, $value, $form, $field ) {
    session_start();
    $pattern = "/^(\d{10})$/";
    if ( $field->type == 'phone' && phoneFormat != 'standard' && ! preg_match( $pattern, $value ) ) {
        $result['is_valid'] = false;
        $result['message']  = 'Please enter a valid phone number it must be 10 digit number';
    }

    return $result;
}

add_filter( 'gform_field_validation_1_5', 'validate_phone_1_5', 10, 4 );
function validate_phone_1_5( $result, $value, $form, $field ) {
    session_start();
    $pattern = "/^(\d{10})$/";
    if ( $field->type == 'phone' && phoneFormat != 'standard' && ! preg_match( $pattern, $value ) ) {
        $result['is_valid'] = false;
        $result['message']  = 'Please enter a valid work number it must be 10 digit number';
    }

    return $result;
}
add_filter( 'gform_field_validation_1_17', 'validate_phone_1_17', 10, 4 );
function validate_phone_1_17( $result, $value, $form, $field ) {
    session_start();
    $pattern = "/^(\d{13})$/";
    if ( $field->type == 'text' && ! preg_match( $pattern, $value ) ) {
        $result['is_valid'] = false;
        $result['message']  = 'Please enter a valid ID Number it must be 13 digit number';
    }

    return $result;
}

add_filter( 'gform_confirmation_1', 'post_to_lead_service', 10, 3 );
function post_to_lead_service( $confirmation, $form, $entry ) {

    $post_url = 'http://www.quickinsure.co.za/LeadService/Service.asmx/SubmitLatestLoanLead';
    $body     = array(
        'UserId' => rgar( $entry, '16' ),
        'Firstname' => rgar( $entry, '1' ),
        'Surname'  => rgar( $entry, '2' ),
        'Email'    => rgar( $entry, '3' ),
        'CellNo'    => rgar( $entry, '4' ),
        'WorkNo'    => rgar( $entry, '5' ),
        'IdNo'    => rgar( $entry, '17' ),
        'NetSalary'    => rgar( $entry, '18' ),
        'GrossSalary'    => rgar( $entry, '19' ),
        'LoanAmount'    => rgar( $entry, '20' ),
        'EmploymentTime'    => rgar( $entry, '11' ),
        'UnderDebtReview'    => rgar( $entry, '21' ),
        'Bank'    => rgar( $entry, '13' ),
        'SubId'    => rgar( $entry, '14' ),
        'TestMode'    => rgar( $entry, '15' ),
    );
    GFCommon::log_debug( 'gform_confirmation: body => ' . print_r( $body, true ) );

    $request  = new WP_Http();
    $response = $request->post( $post_url, array( 'body' => $body ) );
    GFCommon::log_debug( 'gform_confirmation_1: response => ' . print_r( $response, true ) );

    return $confirmation;
}
```
