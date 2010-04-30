About
=====

FreshBooks.rb is a Ruby interface to the FreshBooks API. It exposes easy-to-use classes and methods for interacting with your FreshBooks account.

## Examples ##

## Installation: ##
`  gem install jpablobr-freshbooks.rb`

## Initialization: ##

`  FreshBooks::Base.establish_connection( 'sample.freshbooks.com', 'mytoken' )`

## Creating a client: ##

  def new_client
    client = FreshBooks::Client.new
    @user = User.find(params[:user])
    client.first_name = @user.first_name.nil? @user.first_name : 'first_name'
    client.last_name = @user.last_name.nil? ? @user.last_name : 'last_name'
    client.organization = @user.company.nil? ? @user.company : 'company'
    client.email = @user.email.nil? ? @user.email.nil? : 'email'
    client.create

    unless (client.client_id.nil?)      
      flash[:notice] = "Client with id ##{client.client_id} added to freshbooks successfully"
      @user.update_attributes!(:fb_client_id => client.client_id)
      redirect_to client_path(@user)      
    else
      flash[:notice] = "There was an error!"      
    end
  end

## Send an Invoice: ##

  def send_invoice
    @product = Product.find(params[:product])
    @user = User.find(params[:user])
    @client = FreshBooks::Client.get(@user.fb_client_id)

    invoice = FreshBooks::Invoice.new
    invoice.client_id = 0
    invoice.discount = 0
    invoice.notes = ""
    invoice.currency_code = ""
    invoice.terms = ""
    invoice.lines = []

    @product.items.each do |item|
      line = FreshBooks::Line.new
      line.name = ""
      line.description = ""
      line.unit_cost = 0
      line.quantity = 0
      line.tax1_name = ""
      line.tax1_percent = 0
      invoice.lines << line
    end
    
    invoice.create
    invoice.send_by_email
    flash[:notice] = "Invoice has been sent and emailed to customer successfully"
    redirect_to client_path(@user)
  end

## Updating a client name: ##

  clients = FreshBooks::Client.list
  client = clients[0]
  client.first_name = 'Suzy'
  client.update

## Updating an invoice: ##

  invoice = FreshBooks::Invoice.get(4)
  invoice.lines[0].quantity += 1
  invoice.update

## Creating a new item ##

  item = FreshBooks::Item.new
  item.name = 'A sample item'
  item.create

## Getting an invoice pdf: ##

  invoice = FreshBooks::Invoice.get(4)
  original_status = invoice.status
  invoice.status = "sent"
  raise "API Change or error" unless invoice.update
  html_url = invoice.client_view
  cookiejar = Tempfile.new("cookies")
  pdf_file = Tempfile.new("pdf")
  html_doc = `wget "#{html_url}"  --no-check-certificate --cookies=on --keep-session-cookies --save-cookies='#{cookiejar.path}'  -O -`
  `wget 'https://#{freshbooks_api_host}/getPDF.php' --post-data='invoice_id[]=#{invoice.number}' --cookies=on --keep-session-cookies 
    --load-cookies='#{cookiejar.path}'  --no-check-certificate -O '#{pdf_file.path}'"`
  invoice.status = original_status
  raise "API Change or error" unless invoice.update
  pdf_file.path # Here's the PDF

## License ##

This work is distributed under the MIT License. Use/modify the code however you like.

## Download ##

FreshBooks.rb is distributed as a gem via Rubyforge. The easiest way to install it is like so:

  `gem install freshbooks`

Alternatively, you can download it from the Rubyforge project page.

## Credits ##

[elc](http://elctech.com/tags/freshbooks ) 

FreshBooks.rb is written and maintained by Ben Vinegar, with contributions from Flinn Meuller, Kenneth Kalmer, and others.
