# Maintainer: Mark Wells: mwellsa -at- gmail com
# Contributor: simone riva: siomone.rva -a- gmail com
# Intel Parallel Studio XE 2015 for Linux - ( Intel compiler icc suite )
##########################################################################
# this PKGBUILD splits the main Parallel Studio XE packege in 8 sub-packages:
#
# intel-compiler-base:          Intel C/C++ compiler and base libs
# intel-fortran-compiler:       Intel fortran compiler and base libs"
# intel-openmp:                 Intel OpenMP Library
# intel-idb:                    Intel C/C++ debugger
# intel-ipp:                    Intel Integrated Performance Primitives
# intel-mkl:                    Intel Math Kernel Library (Intel® MKL)
# intel-sourcechecker:          Intel Source Checker
# intel-tbb:                    Intel Threading Building Blocks (TBB)
###########################################################################

# Parallel Studio XE
#     Copyright (C) 2015    Mark Wells
#
#     This program is free software: you can redistribute it and/or modify
#     it under the terms of the GNU General Public License as published by
#     the Free Software Foundation, either version 3 of the License, or
#     (at your option) any later version.
#
#     This program is distributed in the hope that it will be useful,
#     but WITHOUT ANY WARRANTY; without even the implied warranty of
#     MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#     GNU General Public License for more details.
#
#     You should have received a copy of the GNU General Public License
#    along with this program.  If not, see <http://www.gnu.org/licenses/>.

pkgbase="intel-parallel-studio-xe"
pkgname=( 'intel-compiler')

PKGEXT='.pkg.tar.gz'

########################################
#OPTIONS begin
# set to true if you want to remove documentations and examples form the packages.
_remove_docs=false

# set to true if you want to remove the static objects from the libs.
_remove_static_objects_mkl=false
_remove_static_objects_ipp=false
########################################

_year='2016'
_v_a='0'
_v_b='109'

_update=''

pkgrel=1

_sp=''

_studio_ver='16.0.0'

pkgver=${_year}.${_icc_ver}.${_v_a}.${_v_b}

_dir_nr='8001'

options=(strip libtool staticlibs)

url="http://software.intel.com/en-us/articles/non-commercial-software-download/"
arch=('i686' 'x86_64')
license=('custom')
makedepends=('libarchive' 'sed' 'gzip')

_parallel_studio_xe_dir="parallel_studio_xe${_year:+_${_year}}${_sp:+_${_sp}}${_update:+_${_update}}_composer_edition"

source=(
"http://registrationcenter-download.intel.com/akdlm/irc_nas/${_dir_nr}/${_parallel_studio_xe_dir}.tgz"
'intel_compilers.sh'
'intel-composer.install'
'intel-compiler-base.conf'
'intel-fortran.conf'
'intel-openmp.conf'
'intel-mkl.conf'
'intel-ipp.conf'
'intel-tbb.conf'
'intel-mkl.sh'
'intel-mkl-th.conf'
'EULA.txt'
)


sha256sums=(
'f254818ea92b7c2066d8efb6539c376489b7ed121539a4f2732972b6c928f803'  # parallel_studio_xe_2016_composer_edition.tgz
'ab087b1d4e0ccb45ea695cd61a7e8b2817e4d579053c3bfd78658625d65f8ed3'  # intel_compilers.sh
'51f9d94d66aab79129fc51794e32cd5e2abdfd85cd87e70b7a102d746b850257'  # intel-composer.install
'31ac4d0f30a93fe6393f48cb13761d7d1ce9719708c76a377193d96416bed884'  # intel-compiler-base.conf
'c165386ba33b25453d4f5486b7fefcdba7d31e156ad280cbdfa13ed924b01bef'  # intel-fortran.conf
'99cc9683cc75934cc21bb5a09f6ad83365ee48712719bfd914de9444695eed13'  # intel-openmp.conf
'a856326362e9b80c19dc237cbf66bf3d96a69bd7ad1baff99ec9849f8208348c'  # intel-mkl.conf
'da6f41c2e002c9a793c75a18c8d1c85ef7ef5bf83a7a0a158ff144481491aac8'  # intel-ipp.conf
'aee2ae7f87f12f4af38d52423b40d547fd5bbe77e18694b9847e9f2a96d33c6e'  # intel-tbb.conf
'5e68c529c65cac54218026c869e54b2ddb268179725fc1e6b56d920470dad999'  # intel-mkl.sh
'e515cb28bf40cdb0db818db6a2688a0028575153a1b9d5acfb0bc5f13fe45722'  # intel-mkl-th.conf
'228ac25e147adb9b872e1a562e522d2fd48809ccae89b765112009896a6d55a5'  # EULA.txt
)


if [ "$CARCH" = "i686" ]; then
    _i_arch='ia32'
    _i_arch2='i486'

    _not_arch='intel64'
    _not_arch2='x86_64'
else
    _i_arch='intel64'
    _i_arch2='x86_64'

    _not_arch='ia32'
    _not_arch2='i486'
fi


extract_rpms() {
    cd $2
    for rpm_file in ${rpm_dir}/$1 ; do
        echo -e "    Extracting: ${rpm_file}"
        bsdtar -xf ${rpm_file}
    done
}

set_build_vars() {
    _pkg_ver=${_year}.${_icc_ver}.${_v_a}.${_v_b}
    _composer_xe_dir="compilers_and_libraries_${_year}.${_v_a}.${_v_b}"
    rpm_dir=${srcdir}/${_parallel_studio_xe_dir}/rpm
    xe_build_dir=${srcdir}/cxe_build
    base_dir=${srcdir}/..
    _man_dir=${xe_build_dir}/usr/share/man/man1
}


build() {

    set_build_vars

    echo ${xe_build_dir}

    #  clean the builds dirs
    if [ -d ${srcdir}/opt ] ; then
        rm -rf ${srcdir}/opt
    fi

    if [ -d ${srcdir}/etc ] ; then
        rm -rf ${srcdir}/etc
    fi

    if [ -d ${srcdir}/usr ] ; then
        rm -rf ${srcdir}/usr
    fi

    if [ -d ${xe_build_dir} ] ; then
        rm -rf ${xe_build_dir}
    fi

    echo ${srcdir}

    mkdir -p ${xe_build_dir}

    # START !
    cd ${xe_build_dir}
    mkdir -p ${xe_build_dir}/etc/profile.d

    if [ "$CARCH" = "i686" ]; then
        sed 's/<arch>/ia32/' < ${srcdir}/intel_compilers.sh > ${xe_build_dir}/etc/profile.d/intel_compilers.sh
    else
        sed 's/<arch>/intel64/' < ${srcdir}/intel_compilers.sh > ${xe_build_dir}/etc/profile.d/intel_compilers.sh
    fi

    chmod a+x ${xe_build_dir}/etc/profile.d/intel_compilers.sh

    mkdir -p ${xe_build_dir}/etc/ld.so.conf.d

    _cnt=0
    for files in ${base_dir}/*.lic ; do
        _lic_file[_cnt]=${files}
        _cnt=$(($_cnt+1))
    done


    echo -e ""
    echo -e "-----------------------------------------------------------------------------------"
    mkdir -p ${xe_build_dir}/opt/intel/licenses
    if [ -f "${_lic_file[0]}" ]; then
        cp ${base_dir}/*.lic ${xe_build_dir}/opt/intel/licenses
        echo -e "\e[1mFound license files in ${base_dir}."
        echo -e "These will be installed into /opt/intel/licenses ...\e[0m"
    else
        echo -e "\e[1mNo license files found in ${base_dir}."
        echo -e "Remember to place license files in one of these locations:"
        echo -e "    /opt/intel/licenses"
        echo -e "    ~/intel/licenses"
        echo -e "Or the compiler will not work!\e[0m"
    fi
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""

    cp ${srcdir}/${_parallel_studio_xe_dir}/license.txt ${xe_build_dir}/opt/intel/license.txt

    echo -e ""
    echo -e "-----------------------------------------------------------------------------------"
    echo -e "\e[1mIMPORTANT - READ BEFORE COPYING, INSTALLING, OR USING.\e[0m"
    echo -e ""
    echo -e "Do not copy, install, or use the \"Materials\" provided under this license agreement (\"Agreement\"), until you"
    echo -e "have carefully read the following terms and conditions."
    echo -e ""
    echo -e "By copying, installing, or otherwise using the Materials, you agree to be bound by the terms of this"
    echo -e "Agreement. If you do not agree to the terms of this Agreement, do not copy, install, or use the Materials."
    echo -e " - A copy of the EULA has been copied in the PKGBUILD directory; plase read carefully the terms and "
    echo -e " - conditions of the Intel license before installing the packages. "
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""
    echo -e "-----------------------------------------------------------------------------------"
    echo -e " ATTENTION: This PKGBUILD may need up to 20 minutes if you use XZ as a compressor!"
    echo -e "    - The build of the packages: intel-mkl and intel-ipp is particularly slow - "
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""
    echo -e ""
    echo -e "-----------------------------------------------------------------------------------"
    echo -e " \e[1m\e[5mATTENTION: \e[0m \e[1m\e[31mThis PKGBUILD works with yaourt, "
    echo -e "but consumes a lot of RAM! \e[0m "
    echo -e " Using the makepkg command for building this package is recommended."
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""
    echo -e ""
    echo -e "-----------------------------------------------------------------------------------"
    echo -e "\e[1m #################### \e[0m"
    echo -e "\e[1m ##### Options: ##### \e[0m"
    if  ${_remove_docs} ; then
        echo -e " Remove Documentation: YES "
    else
        echo -e " Remove Documentation: NO "
    fi

    if  ${_remove_static_objects_mkl} ; then
        echo -e ""
        echo -e "\e[1m Remove Static Objects from MKL: YES \e[0m \e[1m\e[5m\e[31m ATTENTION !!!! \e[0m "
        echo -e "\e[1m If your software is based on the static objects edit the option at the line 50 of this PKGBUILD \e[0m "
    else
        echo -e " Remove Static Objects: NO "
    fi
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""
    if  ${_remove_static_objects_ipp} ; then
        echo -e ""
        echo -e "\e[1m Remove Static Objects from IPP: YES \e[0m \e[1m\e[5m\e[31m ATTENTION !!!! \e[0m "
        echo -e "\e[1m If your software is based on the static objects edit the option at the line 50 of this PKGBUILD \e[0m "
    else
        echo -e " Remove Static Objects: NO "
    fi
    echo -e "-----------------------------------------------------------------------------------"
    echo -e ""
    echo -e ""


    cd ${xe_build_dir}/opt/intel
    ln -s ./${_composer_xe_dir} composerxe-${_year}
    ln -s ./composerxe-${_year} composerxe

    ln -s ./composerxe/linux/bin/${_i_arch} bin
    ln -s ./composerxe/linux/pkg_bin pkg_bin

    ln -s ./composerxe/linux/ipp/ ipp
    ln -s ./composerxe/linux/compiler/lib/${_i_arch} lib
    ln -s ./composerxe/linux/mkl/ mkl
    ln -s ./composerxe/linux/tbb/ tbb

    _current_dir=`pwd`
    if [ -d ${pkgdir}/opt ] ; then
        cd ${pkgdir}
        rm -rf opt
        cd $_current_dir
    fi ;

    cd  ${srcdir}/${_parallel_studio_xe_dir}/rpm
    rm -v -f *.${_not_arch2}.rpm
    cd $_current_dir
}

package_intel-compiler() {

set_build_vars

pkgdesc="Intel C/C++ compiler"
pkgver=${_pkg_ver}
provides=("intel-tbb" "intel-mkl")
install=intel-composer.install

echo -e " # Start Building"

mkdir -p ${xe_build_dir}/opt
mkdir -p ${xe_build_dir}/etc/profile.d
mkdir -p ${_man_dir}


cp ${srcdir}/intel-compiler-base.conf ${xe_build_dir}/etc/ld.so.conf.d

cd ${xe_build_dir}
echo -e " # Extracting RPMS"

extract_rpms 'intel-*.rpm'  $xe_build_dir

echo -e " # Editing variables"
cd ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/bin

rm *.csh

for f in *.sh ; do
    sed -i 's/<PRODDIR>/\/opt\/intel/g' $f
    sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' $f
done

cd $_i_arch
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' loopprofileviewer.sh
chmod a+x loopprofileviewer.sh
rm loopprofileviewer.csh

cd ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/bin
rm debuggervars.csh
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' debuggervars.sh

cd ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/ipp/bin
rm ippvars.csh
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' ippvars.sh

cd $_i_arch
rm ippvars_${_i_arch}.csh
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' ippvars_${_i_arch}.sh

cd ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/mkl/bin
rm mklvars.csh
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/linux/g' mklvars.sh

rm -rf ./${_not_arch}

cd $_i_arch
rm mklvars_${_i_arch}.csh
sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/g' mklvars_${_i_arch}.sh

echo -e " # Coping man pages"
mv ${xe_build_dir}/opt/intel/documentation_${_year}/en/man/common/man1/*.1 ${_man_dir}

cd ${_man_dir}
for f in *.1 ; do
    gzip $f
done

cd ${xe_build_dir}

cp ${srcdir}/intel-compiler-base.conf ${xe_build_dir}/etc/ld.so.conf.d

if [ "$CARCH" = "i686" ]; then
    sed 's/<arch>/ia32/' < ${srcdir}/intel-fortran.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-fortran.conf
else
    sed 's/<arch>/intel64/' < ${srcdir}/intel-fortran.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-fortran.conf
fi

cp ${srcdir}/intel-openmp.conf ${xe_build_dir}/etc/ld.so.conf.d

if [ "$CARCH" = "i686" ]; then
    sed 's/<arch>/ia32/' < ${srcdir}/intel-ipp.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-ipp.conf
else
    sed 's/<arch>/intel64/' < ${srcdir}/intel-ipp.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-ipp.conf
fi

if [ "$CARCH" = "i686" ]; then
    sed 's/<arch>/ia32/' < ${srcdir}/intel-tbb.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-tbb.conf
    sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/g' ${xe_build_dir}/etc/ld.so.conf.d/intel-tbb.conf
else
    sed 's/<arch>/intel64/' < ${srcdir}/intel-tbb.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-tbb.conf
    sed -i 's/<INSTALLDIR>/\/opt\/intel\/composerxe/g' ${xe_build_dir}/etc/ld.so.conf.d/intel-tbb.conf
fi

cp ${srcdir}/intel-mkl.sh ${xe_build_dir}/etc/profile.d
chmod a+x ${xe_build_dir}/etc/profile.d/intel-mkl.sh

cp ${srcdir}/intel-mkl-th.conf ${xe_build_dir}/etc/

if [ "$CARCH" = "i686" ]; then
    sed 's/<arch>/ia32/' < ${srcdir}/intel-mkl.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-mkl.conf
else
    sed 's/<arch>/intel64/' < ${srcdir}/intel-mkl.conf > ${xe_build_dir}/etc/ld.so.conf.d/intel-mkl.conf
fi

if $_remove_docs ; then
    echo -e " # Remove docs"
    rm -rf ${xe_build_dir}/opt/intel/documentation_${_year}
    rm -rf ${xe_build_dir}/opt/intel/samples_${_year}
fi

if ${_remove_static_objects_ipp} ; then
    echo -e " # intel-ipp: Remove static objects"
    rm -f ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/ipp/lib/${_i_arch}/libipp*.a
    rm -f ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/ipp/lib/mic/libipp*.a
fi

if ${_remove_static_objects_mkl} ; then
    echo -e " # intel-mkl: remove static objects"
    rm -f ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/mkl/lib/${_i_arch}/libmkl_*.a
    rm -f ${xe_build_dir}/opt/intel/${_composer_xe_dir}/linux/mkl/lib/mic/libmkl_*.a
fi

echo -e " # Move package"
mv ${xe_build_dir}/opt ${pkgdir}
mv ${xe_build_dir}/etc ${pkgdir}
mv ${xe_build_dir}/usr ${pkgdir}
}


pkgdesc="Intel C++ C and fortran compiler - Intel Parallel Studio XE  - intel compiler - icc icpc ifort ipp mkl "
depends=('bash' 'gcc')
