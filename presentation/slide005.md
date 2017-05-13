            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Aisladas de otros objectos en el mismo sistema, incluyendo la BD

        Factory.define :payment do |p|
          p.status       :paid
          p.association  :account, factory: :payment_account
          p.association  :client
          p.email        'john@doe.com'
          p.phone_number '529999999999'
          p.disbursed_at  Date.current
        end

        test 'payment is invalid if wrong telephone on client' do
          payment = Factory.build :payment, client: nil, phone_number: 'abc123'
          payment.stubs(:processed_payment?)
          payment.save
          assert payment.invalid?
        end

        test 'some_test' do
          3.times { Factory.create :payment }
          expected_payment = Factory.create :payment, attribute: 'value 1'
          assert_equal 4, Payment.count
          assert_equal expected_payment, Payment.some_scope
        end
















































































slide 005
