*   Fix year value when casting a multiparameter time hash

    When assigning a hash to a time attribute that's missing a year component
    (e.g. a `time_select` with `:ignore_date` set to `true`) then the year
    defaults to 1970 instead of the expected 2000. This results in the attribute
    changing as a result of the save.

    Before:
    ```
    event = Event.new(start_time: { 4 => 20, 5 => 30 })
    event.start_time # => 1970-01-01 20:30:00 UTC
    event.save
    event.reload
    event.start_time # => 2000-01-01 20:30:00 UTC
    ```

    After:
    ```
    event = Event.new(start_time: { 4 => 20, 5 => 30 })
    event.start_time # => 2000-01-01 20:30:00 UTC
    event.save
    event.reload
    event.start_time # => 2000-01-01 20:30:00 UTC
    ```

    *Andrew White*


## Rails 6.0.0.beta1 (January 18, 2019) ##

*   Add `ActiveModel::Errors#of_kind?`.

    *bogdanvlviv*, *Rafael Mendonça França*

*   Fix numericality equality validation of `BigDecimal` and `Float`
    by casting to `BigDecimal` on both ends of the validation.

    *Gannon McGibbon*

*   Add `#slice!` method to `ActiveModel::Errors`.

    *Daniel López Prat*

*   Fix numericality validator to still use value before type cast except Active Record.

    Fixes #33651, #33686.

    *Ryuta Kamizono*

*   Fix `ActiveModel::Serializers::JSON#as_json` method for timestamps.

    Before:
    ```
    contact = Contact.new(created_at: Time.utc(2006, 8, 1))
    contact.as_json["created_at"] # => 2006-08-01 00:00:00 UTC
    ```

    After:
    ```
    contact = Contact.new(created_at: Time.utc(2006, 8, 1))
    contact.as_json["created_at"] # => "2006-08-01T00:00:00.000Z"
    ```

    *Bogdan Gusiev*

*   Allows configurable attribute name for `#has_secure_password`. This
    still defaults to an attribute named 'password', causing no breaking
    change. There is a new method `#authenticate_XXX` where XXX is the
    configured attribute name, making the existing `#authenticate` now an
    alias for this when the attribute is the default 'password'.

    Example:

        class User < ActiveRecord::Base
          has_secure_password :recovery_password, validations: false
        end

        user = User.new()
        user.recovery_password = "42password"
        user.recovery_password_digest # => "$2a$04$iOfhwahFymCs5weB3BNH/uX..."
        user.authenticate_recovery_password('42password') # => user

    *Unathi Chonco*

*   Add `config.active_model.i18n_full_message` in order to control whether
    the `full_message` error format can be overridden at the attribute or model
    level in the locale files. This is `false` by default.

    *Martin Larochelle*

*   Rails 6 requires Ruby 2.5.0 or newer.

    *Jeremy Daer*, *Kasper Timm Hansen*


Please check [5-2-stable](https://github.com/rails/rails/blob/5-2-stable/activemodel/CHANGELOG.md) for previous changes.
