            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Validan todos los posibles escenarios in/out

        Factory.define :invoice, class: Invoice do |i|
          i.name         'Some Person Requesting'
          i.rfc          'SPR120436ABC'
          i.zipcode      22000
          i.invoice_date 3.days.ago
          i.association  :payment
          i.folio        { Invoice.last_folio + 1 }
        end

        test 'Invoice is_valid? method returns true if there is a successful gface_response' do
          invoice = Factory.build :guatemala_invoice, gface_response: successful_gface_response
          assert_equal true, invoice.is_valid?
        end

        test 'Invoice is_valid? method returns false if there is an unsuccessful gface_response' do
          invoice = Factory.build :guatemala_invoice, gface_response: unsuccessful_gface_response
          assert_equal false, invoice.is_valid?
        end

        test 'Invoice is_valid? method returns false if there is no gface_response stored' do
          invoice = Factory.build :guatemala_invoice, gface_response: {}
          assert_equal false, invoice.is_valid?
        end
















































































slide 007
