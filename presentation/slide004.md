            ____                  __                              _ __             _
           / __ \_______  _____  / /_  ____ ______   __  ______  (_) /_____ ______(_)___ ______
          / /_/ / ___/ / / / _ \/ __ \/ __ `/ ___/  / / / / __ \/ / __/ __ `/ ___/ / __ `/ ___/
         / ____/ /  / /_/ /  __/ /_/ / /_/ (__  )  / /_/ / / / / / /_/ /_/ / /  / / /_/ (__  )
        /_/   /_/   \__,_/\___/_.___/\__,_/____/   \__,_/_/ /_/_/\__/\__,_/_/  /_/\__,_/____/

        • Corren de forma aislada y aleatoria

        Aisladas de otros sistemas (stubs)

        def setup
          FakeWeb.allow_net_connect = false
          FakeWeb.register_uri :post, /#{ITIMBRE_CONFIG[:uri][Rails.env.to_sym]}/, body: '', parameters: :any
          @itimbre = Itimbre::Itimbre.new 1, sample_xml, nil, CURRENT_ISSUER
        end

        test "success" do
          Itimbre::ItimbreResponse.any_instance.stubs(:result).returns(fake_success_response)
          assert_equal sample_xml, @itimbre.stamp
        end
















































































slide 004
