#!/usr/bin/env bash

# Copyright (c) 2015, Frédéric Fauberteau
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
# 1. Redistributions of source code must retain the above copyright notice,
#    this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution.
# 3. Neither the name of the copyright holder nor the names of its
#    contributors may be used to endorse or promote products derived from this
#    software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.

function error ()
{
  msg=$1
  echo "Usage: $0 INFILE [OUTFILE]"
  if [ "${msg}" ] ; then
    echo "$0: ${msg}"
  fi
  exit 1
}

if [ $# -lt 1 ] ; then
  error
fi
infile=$1
if [ ! -f ${infile} ] ; then
  error "no such file"
fi
if [ $(which markdown 2> /dev/null) ] ; then
  markdown=$(which markdown)
else
  error "'markdown' command is needed"
fi
if [ $(which wkhtmltopdf 2> /dev/null) ] ; then
  wkhtmltopdf=$(which wkhtmltopdf)
else
  error "'wkhtmltopdf' command is needed"
fi
intype=$(file -b --mime-type ${infile})
inenc=$(file -b --mime-encoding ${infile})
inext=${infile##*.}
if [ "x${intype}" != "xtext/plain" ] ; then
  error "mime type of input file must be 'text/plain'"
fi
if [ $# -eq 2 ] ; then
  outfile=$2
else
  if [ "x${inext}" = "x${infile}" ] ; then
    outfile=${infile}
  else
    outfile=${infile%.${inext}}
  fi
  outfile=${outfile}.pdf
fi
tmpfile=$(mktemp XXXXXX.html)
${markdown} ${infile} > ${tmpfile}
if [ "x${inenc}" = "xutf-8" ] ; then
  sed -i '1s/^/<meta charset="UTF-8">/' ${tmpfile}
fi
${wkhtmltopdf} -B 10 -T 10 ${tmpfile} ${outfile} 2> /dev/null
rm -f ${tmpfile}
