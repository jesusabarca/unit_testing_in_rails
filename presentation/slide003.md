            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Validan comportamiento, no implementación (ni el framework)

        Factory.define :invoice, class: Invoice do |i|
          i.name         'Some Person Requesting'
          i.rfc          'SPR120436ABC'
          i.zipcode      22000
          i.invoice_date 3.days.ago
          i.association  :payment
          i.folio        { Invoice.last_folio + 1 }
        end

        class Invoice < ActiveRecord::Base
          ...

          private

          def decimal_format(integer)
            ...
          end
        end

        test "decimal format should fill with 0 when not a float" do
          assert invoice = Invoice.new
          assert_equal "10.00", invoice.send(:decimal_format, 10)
        end
















































































slide 003
