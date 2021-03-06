
bin_PROGRAMS = nfcapd nfdump nfreplay nfexpire nfanon
check_PROGRAMS = nftest nfgen nfreader

EXTRA_DIST = applybits_inline.c nffile_inline.c collector_inline.c inline.c nfdump_inline.c heapsort_inline.c test.sh nfdump.test.out nfdump.test.diff

check_PROGRAMMS = test.sh
TESTS = nftest test.sh

if SFLOW
bin_PROGRAMS += sfcapd
endif

if NFPROFILE
bin_PROGRAMS += nfprofile
endif

if NFTRACK
bin_PROGRAMS += nftrack
endif

if FT2NFDUMP
bin_PROGRAMS += ft2nfdump
endif

BUILT_SOURCES = grammar.h
AM_YFLAGS = -d

AM_CPPFLAGS = $(DEPS_CFLAGS)
LDADD = $(DEPS_LIBS)

#Add extra debug info for gdb
AM_CFLAGS = -ggdb 

# libnfdump sources
output =  output_util.h output_util.c output_raw.h output_raw.c 
output += output_json.c output_json.h output_csv.c output_csv.h output_pipe.c output_pipe.h
output += output_fmt.c output_fmt.h 
util = util.c util.h
filelzo = minilzo.c minilzo.h lzoconf.h lzodefs.h lz4.c lz4.h nffile.c nffile.h nfx.c nfx.h 
nflist = flist.c flist.h fts_compat.c fts_compat.h
filter = grammar.y scanner.l nftree.c nftree.h ipconv.c ipconv.h rbtree.h
exporter = exporter.c exporter.h

nfprof = nfprof.c nfprof.h
nfnet = nfnet.c nfnet.h
anon = panonymizer.c panonymizer.h rijndael.c rijndael.h
collector = collector.c collector.h
nfv1 = netflow_v1.c netflow_v1.h
nfv9 = netflow_v9.c netflow_v9.h
# pcaproc = pcaproc.c pcaproc.h flowtree.c flowtree.h ipfrag.c ipfrag.h malloc_hook.c
pcaproc = pcaproc.c pcaproc.h flowtree.c flowtree.h ipfrag.c ipfrag.h
content = content_dns.c content_dns.h
netflow_pcap = netflow_pcap.c netflow_pcap.h
ipfix = ipfix.c ipfix.h
nfv5v7 = netflow_v5_v7.c netflow_v5_v7.h
nfstatfile = nfstatfile.c nfstatfile.h
nflowcache = nflowcache.c nflowcache.h
bookkeeper = bookkeeper.c bookkeeper.h
expire= expire.c expire.h
launch = launch.c launch.h

lib_LTLIBRARIES = libnfdump.la
libnfdump_la_SOURCES = $(output) $(util) $(filelzo) $(nflist) $(filter) $(exporter)
libnfdump_la_LDFLAGS = -release 1.6.19


nfdump_SOURCES = nfdump.c nfdump.h nfstat.c nfstat.h nfexport.c nfexport.h  \
	$(nflowcache) $(nfprof)
nfdump_LDADD = -lnfdump
nfdump_DEPENDENCIES = libnfdump.la

nfreplay_SOURCES = nfreplay.c $(nfprof) \
	$(nfnet) $(collector) $(nfv1) $(nfv9) $(nfv5v7) $(ipfix)
nfreplay_LDADD = -lnfdump
nfreplay_DEPENDENCIES = libnfdump.la

nfprofile_SOURCES = nfprofile.c profile.c profile.h $(nfstatfile) 
nfprofile_LDADD = -lnfdump -lrrd
nfprofile_DEPENDENCIES = libnfdump.la

nftrack_SOURCES = ../extra/nftrack/nftrack.c \
	../extra/nftrack/nftrack_rrd.c ../extra/nftrack/nftrack_rrd.h \
	../extra/nftrack/nftrack_stat.c ../extra/nftrack/nftrack_stat.h 
nftrack_CFLAGS = -I ../extra/nftrack
nftrack_LDADD = -lnfdump -lrrd
nftrack_DEPENDENCIES = libnfdump.la

nfcapd_SOURCES = nfcapd.c \
	$(nfstatfile) $(launch) \
	$(nfnet) $(collector) $(nfv1) $(nfv5v7) $(nfv9) $(ipfix) $(bookkeeper) $(expire)
nfcapd_LDADD = -lnfdump 
nfcapd_DEPENDENCIES = libnfdump.la

nfpcapd_SOURCES = nfpcapd.c \
	$(pcaproc) $(netflow_pcap) \
	$(nfstatfile) $(launch) \
	$(nfnet) $(collector) $(bookkeeper) $(expire) $(content)
nfpcapd_LDADD = -lnfdump 
nfpcapd_DEPENDENCIES = libnfdump.la

if READPCAP
nfcapd_CFLAGS = -DPCAP
nfcapd_SOURCES += pcap_reader.c pcap_reader.h
nfcapd_LDADD += -lpcap 
endif

if BUILDNFPCAPD
bin_PROGRAMS += nfpcapd
nfpcapd_CFLAGS = -D_BSD_SOURCE -D_DEFAULT_SOURCE
nfpcapd_LDADD += -lpcap 
nfpcapd_LDFLAGS = -pthread
endif

sfcapd_SOURCES = sfcapd.c sflow_nfdump.c sflow_nfdump.h sflow.h sflow_v2v4.h sflow_process.c  sflow_process.h\
	$(nfstatfile) $(launch) \
	$(nfnet) $(collector) $(bookkeeper) $(expire)
sfcapd_LDADD = -lnfdump 
sfcapd_DEPENDENCIES = libnfdump.la

if READPCAP
sfcapd_CFLAGS = -DPCAP
sfcapd_SOURCES += pcap_reader.c pcap_reader.h
sfcapd_LDADD += -lpcap
endif

nfreader_SOURCES = nfreader.c 
nfreader_LDADD = -lnfdump 
nfreader_DEPENDENCIES = libnfdump.la

nfanon_SOURCES = nfanon.c $(anon)
nfanon_LDADD = -lnfdump 
nfanon_DEPENDENCIES = libnfdump.la

nfgen_SOURCES = nfgen.c 
nfgen_LDADD = -lnfdump 
nfgen_DEPENDENCIES = libnfdump.la

nfexpire_SOURCES = nfexpire.c \
	$(bookkeeper) $(expire) $(nfstatfile)
nfexpire_LDADD = -lnfdump @FTS_OBJ@
nfexpire_DEPENDENCIES = libnfdump.la

nftest_SOURCES = nftest.c 
nftest_LDADD = -lnfdump 
nftest_DEPENDENCIES = nfgen libnfdump.la

if FT2NFDUMP
ft2nfdump_SOURCES = ft2nfdump.c 
ft2nfdump_CFLAGS = @FT_INCLUDES@
ft2nfdump_LDADD = -lnfdump -lft -lz
ft2nfdump_LDADD += @FT_LDFLAGS@
ft2nfdump_DEPENDENCIES = libnfdump.la
endif

check_DIST = inline.c collector_inline.c nffile_inline.c nfdump_inline.c heapsort_inline.c applybits_inline.c 
check_DIST += test.sh nfdump.test.out parse_csv.pl AddExtension.txt	nfdump.test.diff
CLEANFILES = lex.yy.c grammar.c grammar.h scanner.c scanner.h $(check_PROGRAMS) *.gch
