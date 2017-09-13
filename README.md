# ORCFILE
Ruby Gem for creating and reading Optimized Row Columnar (ORC) files.
This gem can also be paired using the dif_fileio and/or factory_girl.

## Installation
Must use jruby.

Add this line to your application's Gemfile:

    gem 'orcfile'

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install orcfile

## Usage
###OrcFileWriter
To write a file, you will need to initialize the OrcFileWriter class.
This object needs a table schema, your dataset, the path to store the file, and an optional configuration hash.
    
    OrcFileWriter.new(table_schema, data_set, path, *options={}) 
####*table_schema*
The table_schema must be a hash containing the column name and datatype as the key-value pair.
      
Valid datatypes are:    
- integer 
- decimal
- float
- date
- datetime
- time
- string  

    
    table_schema = {:id => :integer, :amount => :decimal, :rate => :float}
    
####*data_set*
The data_set must contain a hash with the column name and data value as the key-value pair.

For one row in the dataset:

    data_set = {:id => 1, :amount => 1000.01, :rate => 0.0005}
    
For multiple rows in the dataset:

    dataset = [{:id => 1, :amount => 1000.01, :rate => 0.0005},
               {:id => 2, :amount => 2500.5, :rate => 0.1},
               {:id => 3, :amount => 10.12, :rate => 10.0134}]

####*path*
The path should be the full file path or relative to your working directory. You must also specify the file name.

    path = '/temp/orc_file.orc'
    
####*options*
Options is an optional hash parameter containing 5 configurable settings for writing an ORC file.
   
   `:stripe_size` defines the size of the stripe, defaulted as 67,108,864 bytes <br>
   `:row_index_stride` defines the number of rows between row index entries, defaulted as 10,000 <br>
   `:buffer_size` defines the orc buffer size, defaulted as 262,144 bytes <br>
   `:compression` defines the compression codec (NONE,ZLIB,SNAPPY,LZO), defaulted as ZLIB. <br>
    
Define the options parameter has a hash
    
    options = {:stripe_size => 70000000, :compression => 'SNAPPY'}
  
###write_to_orc
Once you have the OrcFileWriter object initialized you must call write_to_orc to write out the file

      OrcFileWriter.new(table_schema, data_set, path, options).write_to_orc

