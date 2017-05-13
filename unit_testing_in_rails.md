#Pruebas unitarias en
##Ruby on Rails

![Ruby](img/ruby-on-rails.png)


#Pruebas unitarias

- MiniTest
- Fixtures/FactoryGirl
- Minitest::Mock/Mocha


#Pruebas unitarias

- Validan comportamiento, no implementación (ni el framework)

```ruby
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
```


#Pruebas unitarias

- Corren de forma aislada y aleatoria

Aisladas de otros sistemas (stubs)

```ruby
def setup
  FakeWeb.allow_net_connect = false
  FakeWeb.register_uri :post, /#{ITIMBRE_CONFIG[:uri][Rails.env.to_sym]}/, body: '', parameters: :any
  @itimbre = Itimbre::Itimbre.new 1, sample_xml, nil, CURRENT_ISSUER
end

test "success" do
  Itimbre::ItimbreResponse.any_instance.stubs(:result).returns(fake_success_response)
  assert_equal sample_xml, @itimbre.stamp
end
```


#Pruebas unitarias

- Aisladas de otros objectos en el mismo sistema, incluyendo la BD

```ruby
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
```

```ruby
test 'some_test' do
  3.times { Factory.create :payment }
  expected_payment = Factory.create :payment, attribute: 'value 1'
  assert_equal 4, Payment.count
  assert_equal expected_payment, Payment.some_scope
end
```


#Pruebas unitarias

- Son expresivas y describen su contexto

```ruby
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
```


#Pruebas unitarias

- Validan todos los posibles escenarios in/out

```ruby
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
```


#Pruebas unitarias

- Cumplen expectativas (mocks)

```ruby
test 'can execute custom query' do
  estafeta = Couriers::Estafeta::EstafetaShipping.new
  request = {foo: :bar}
  estafeta.client.expects(:call)
  estafeta.execute_query request
end
```


#Links

- http://guides.rubyonrails.org/testing.html
- https://github.com/seattlerb/minitest
- https://www.youtube.com/watch?v=RAxiiRPHS9k
- https://github.com/freerange/mocha
- https://github.com/thoughtbot/factory_girl
