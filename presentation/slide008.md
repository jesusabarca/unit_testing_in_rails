            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Cumplen expectativas (mocks)

        test 'can execute custom query' do
          estafeta = Couriers::Estafeta::EstafetaShipping.new
          request = {foo: :bar}
          estafeta.client.expects(:call)
          estafeta.execute_query request
        end
















































































slide 008
