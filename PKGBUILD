# -*- Mode: shell-script; sh-basic-offset: 2 -*-
# Contributor: minimalist <minimalist@lavabit.com>
pkgname='perl-mongodb-nodatetime'
pkgver='20110908'
pkgrel='1'
pkgdesc="MongoDB's Perl driver that doesn't use Moose and DateTime"
arch=('i686' 'x86_64')
license=('Apache' 'GPL')
options=('!emptydirs')
depends=('perl>=5.8.7')
makedepends=('perl-boolean' 'perl-data-types' 'perl-datetime' 'perl-file-slurp' 'perl-json' 'perl-test-exception' 'perl-tie-ixhash' 'perl-try-tiny')
provides=('perl-mongodb-nodatetime')
url='https://github.com/naturalist/mongo-perl-driver'
md5sums=()
source=()
conflicts=('perl-mongodb')

_gitroot='git://github.com/naturalist/mongo-perl-driver.git'
_gitbranch=${BRANCH:-'master'}
_name='MongoDB';

build() {
  DIST_DIR="${srcdir}/${pkgname}"
  msg "Creating $_name ..."

  if [ -d "$DIST_DIR" ] ; then
    warning 'Repository directory already exists!'
    msg2 'Attempting to pull from repo...'
    cd "$DIST_DIR"
    git pull origin "$_gitbranch"
  else
    msg2 "Cloning $_gitroot repository..."
    git clone "$_gitroot" "$DIST_DIR"
    cd "$DIST_DIR"
  fi

  msg2 "Checking out the $_gitbranch branch..."
  git checkout "$_gitbranch"
  if [ "$?" -ne 0 ] ; then
    error "Failed to checkout the $_gitbranch branch... aborting."
    return 1
  fi

  export PERL_MM_OPT="INSTALLDIRS=vendor DESTDIR=$pkgdir"  \
    PERL_MB_OPT="--installdirs vendor --destdir '$pkgdir'" \
    MODULEBUILDRC='/dev/null' TEST_RELEASE=1

  msg "Building $_name..."
  { cd "$DIST_DIR"   &&
    perl Makefile.PL &&
    make             &&
    msg2 "Testing $_name..." &&
    make test        &&
    make install     ;
  } || return 1;

  find "$pkgdir" -name .packlist -o -name perllocal.pod -delete
}
