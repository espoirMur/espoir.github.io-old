find replace the pyhtonic write_of_or_self_payed

data['attribute']

with data.get('attribute')

find data\[(\S+)\]

data : data
\[ : data[ wit
\] : data ]

() : group

\S+ white space characheter

replace

To reference a capture, use $n where n is the capture register number.
Using $0 means the entire match.

data.get($1)
