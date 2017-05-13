            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Son expresivas y describen su contexto

        Factory.define :daily_exchange_rate do |d|
          d.base_currency 'MXN'
          d.quote_currency 'USD'
          d.quoted_at Date.today
          d.rate 0.5
        end

        Factory.define :usd_exchange_rate, parent: :daily_exchange_rate do |d|
          d.base_currency 'USD'
          d.rate 1
        end

        test 'a guatemala_invoice in USD returns the reciprocal of the exchange rate GTQ-USD' do
          Factory.create :usd_exchange_rate
          account = Factory.create :guatemala_dollar_account
          payment = Factory.create :guatemala_payment, account: account
          invoice = Factory.create :guatemala_invoice, payment: payment
          rate = DailyExchangeRate.currency('GTQ').for_date(invoice.created_at).first
          assert_equal (1 / rate.exchange_rate), invoice.exchange_rate
        end
















































































slide 006
