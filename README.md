# Snapshot of the NYPD Podcast site

You can view the mirrored version by visiting this URL:

http://wgetsnaps.github.io/nyc.gov--nypd-podcasts/www.nyc.gov/html/nypd/html/pr/podcasts.shtml.html


This repository is a mirror of the following site:

|        Key         |                 Value                  |
|--------------------|----------------------------------------|
| Site title         | Death Row Information                  |
| Original publisher | Texas Dept. of Criminal Justice        |
| URL                | http://www.tdcj.state.tx.us/death_row/ |
| Mirrored at        | 2016-01-11 07:16:39                    |





# Bash script and wget

Here are the Bash commands and __wget__ invocations I used to mirror the site. I use __sed__ to do some in-place replacements of absolute addresses in the XML file.


~~~sh
wget --recursive   \
     --span-hosts  \
     --level 1     \
     --adjust-extension  \
     --convert-links   \
     --output-file /dev/stdout \
     http://www.nyc.gov/html/nypd/html/pr/podcasts.shtml |  
     tee -a ./wget.log
# get xml file
wget --force-directories \
     --output-file /dev/stdout \
     http://www.nyc.gov/html/nypd/audio/podcast_nycgov.xml |
     tee -a ./wget.log

# edit HTML to point to local XML file
sed -i '.bak' 's@http://www.nyc.gov/html/nypd/audio/podcast_nycgov.xml@../../audio/podcast_nycgov.xml@g' \
    ./www.nyc.gov/html/nypd/html/pr/podcasts.shtml.html \
    ./www.nyc.gov/html/nypd/html/pr/press_room.shtml.html

# edit XML to have relative addresses

sed -i '.bak' 's@http://www.nyc.gov/html/nypd/@../@g' \
    ./www.nyc.gov/html/nypd/audio/podcast_nycgov.xml 

~~~
