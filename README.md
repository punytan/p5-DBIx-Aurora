# NAME

DBIx::Aurora - DBI handler specialized in Amazon Aurora

# SYNOPSIS

    use DBIx::Aurora;

    my $aurora = DBIx::Aurora->new(
        CLUSTER_1 => { # AUTOLOAD method named `cluster_1`
            instances => [
                [ $dsn_1, $user, $password, $attr ],
                [ $dsn_2, $user, $password, $attr ],
                [ $dsn_3, $user, $password, $attr ],
            ],
            opts => {
                force_reader_only  => 0,
                reconnect_interval => 3600,
                logger => sub {
                    my ($error_type, $message, $exception) = @_;
                    ...;
                },
            }
        },
        CLUSTER_2 => { # AUTOLOAD method named `cluster_2`
            instances => [
                [ $dsn_4, $user, $password, $attr ],
                [ $dsn_5, $user, $password, $attr ],
                [ $dsn_6, $user, $password, $attr ],
            ],
            opts => {
                force_reader_only  => 0,
                reconnect_interval => 3600,
                logger => sub {
                    my ($error_type, $message, $exception) = @_;
                    ...;
                },
            }
        },
    );

    my $rv = eval {
        $aurora->cluster_1->writer(sub {
            my $dbh = shift;
            $dbh->do($query_1, undef, @bind_1);
        });
    };
    if (my $e = $@) {
        logger->crit("Failed to execute query: $e");
    }

    my $row = eval {
        $aurora->cluster_2->reader(sub {
          my $dbh = shift;
          $dbh->selectrow_hashref($queruy_2, undef, @bind_2)
      });
    };
    if (my $e = $@) {
        logger->crit("Failed to execute query: $e");
    }

# DESCRIPTION

DBIx::Aurora is a DBI handler specialized in Amazon Aurora.

`DBIx::Aurora` detects writer/reader instances automatically and manages network connections. Also you can handle multiple Aurora clusters.

# METHOD

## `AUTOLOAD`ed methods

You can access registered clusters and/or instances via AUTOLOADed methods. It returns [DBIx::Aurora::Cluster](https://metacpan.org/pod/DBIx::Aurora::Cluster) instance.

# AUTHOR

punytan <punytan@gmail.com>

# COPYRIGHT

Copyright 2017- punytan

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
