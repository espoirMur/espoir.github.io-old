find replace in atom 

The problem 
we need to replace data['attribute'] with data.get('attribute') in atom text editor or another test editor.

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
