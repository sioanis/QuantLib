# Process this file with autoconf to produce a configure script.
AC_INIT([QuantLib], [1.26-dev],
        [quantlib-dev@lists.sourceforge.net],
        [QuantLib])
AC_PREREQ(2.62)
AC_CONFIG_SRCDIR([ql/qldefines.hpp])
AC_CONFIG_AUX_DIR([config])
AC_CONFIG_HEADERS([ql/config.hpp])
AM_INIT_AUTOMAKE([foreign nostdinc subdir-objects tar-ustar])
AC_CONFIG_MACRO_DIR([m4])

AC_COPYRIGHT([
Copyright (C) 2000, 2001, 2002, 2003 RiskMap srl
Copyright (C) 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010 StatPro Italia srl
Copyright (C) 2004, 2005 Ferdinando Ametrano

This file is part of QuantLib, a free-software/open-source library
for financial quantitative analysts and developers - http://quantlib.org/

QuantLib is free software: you can redistribute it and/or modify it
under the terms of the QuantLib license.  You should have received a
copy of the license along with this program; if not, please email
<quantlib-dev@lists.sf.net>. The license is also available online at
<http://quantlib.org/license.shtml>.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE.  See the license for more details.])

AC_PROG_AWK
AC_PROG_INSTALL

# Help the compiler find external software

AC_ARG_WITH([boost-include],
            AS_HELP_STRING([--with-boost-include=INCLUDE_PATH],
                           [Supply the location of Boost header files]),
            [ql_boost_include_path="`cd ${withval} 2>/dev/null && pwd`"],
            [ql_boost_include_path=""])
if test [ -n "$ql_boost_include_path" ] ; then
   AC_SUBST([BOOST_INCLUDE],["-I${ql_boost_include_path}"])
   AC_SUBST([CPPFLAGS],["${CPPFLAGS} -I${ql_boost_include_path}"])
fi
AC_ARG_WITH([boost-lib],
            AS_HELP_STRING([--with-boost-lib=LIB_PATH],
                           [Supply the location of Boost libraries]),
            [ql_boost_lib_path="`cd ${withval} 2>/dev/null && pwd`"],
            [ql_boost_lib_path=""])
if test [ -n "$ql_boost_lib_path" ] ; then
   AC_SUBST([BOOST_LIB],["-L${ql_boost_lib_path}"])
   AC_SUBST([LDFLAGS],["${LDFLAGS} -L${ql_boost_lib_path}"])
fi


# Continue setup

AC_PROG_CC
AC_PROG_CXX
AC_PROG_CPP
LT_INIT

AM_CONDITIONAL(ENABLE_STATIC, test "${enable_static}" == "yes")

AC_LANG([C++])

AC_ARG_ENABLE([openmp],
              AS_HELP_STRING([--enable-openmp],
                             [If enabled, configure will try to detect
                              and enable OpenMP support.]),
              [ql_openmp=$enableval],
              [ql_openmp=no])
if test "$ql_openmp" = "yes" ; then
   AC_OPENMP
   AC_SUBST([CXXFLAGS],["${CXXFLAGS} ${OPENMP_CXXFLAGS}"])
fi

# Check for C++11 support
QL_CHECK_CPP11

# Check for Boost components
QL_CHECK_BOOST
AM_CONDITIONAL(BOOST_UNIT_TEST_FOUND, test "x${BOOST_UNIT_TEST_LIB}" != "x")

# Check for Emacs
AM_PATH_LISPDIR
AM_CONDITIONAL(EMACS_FOUND, test "${EMACS}" != "no")

# Check for tools needed for building documentation
AC_PATH_PROG([DOXYGEN], [doxygen])
AC_PATH_PROG([SED], [sed])
AC_PATH_PROG([LATEX], [latex])
AC_PATH_PROG([PDFLATEX], [pdflatex])
AC_PATH_PROG([MAKEINDEX], [makeindex])
AC_PATH_PROG([DVIPS], [dvips])


# site configuration

# AC_ARG_WITH([real],
#             AS_HELP_STRING([--with-real=type],
#                            [The given type will be used for floating-point
#                             calculations.
#                             In case this option is not invoked, double
#                             will be used.]),
#             [AC_DEFINE_UNQUOTED([QL_REAL],[$withval],
#                                 [Define this as the type to be used for
#                                  floating-point calculations.])],
#             [AC_DEFINE_UNQUOTED([QL_REAL],[double],
#                                 [Define this as the type to be used for
#                                  floating-point calculations.])])

# AC_ARG_WITH([integer],
#             AS_HELP_STRING([--with-integer=type],
#                            [The given type will be used for integer
#                             calculations. At least an int is required.
#                             In case this option is not invoked, int
#                             will be used.]),
#             [AC_DEFINE_UNQUOTED([QL_INTEGER],[$withval],
#                                 [Define this as the type to be used for
#                                  integer calculations. At least an int
#                                  is required.])],
#             [AC_DEFINE_UNQUOTED([QL_INTEGER],[int],
#                                 [Define this as the type to be used for
#                                  integer calculations. At least an int
#                                  is required.])])

# AC_ARG_WITH([big-integer],
#             AS_HELP_STRING([--with-big-integer=type],
#                            [The given type will be used for large integer
#                             calculations. At least 4 bytes are required.
#                             In case this option is not invoked, long
#                             will be used.]),
#             [AC_DEFINE_UNQUOTED([QL_BIG_INTEGER],[$withval],
#                                 [Define this as the type to be used for
#                                  large integer calculations. At least
#                                  4 bytes are required.])],
#             [AC_DEFINE_UNQUOTED([QL_BIG_INTEGER],[long],
#                                 [Define this as the type to be used for
#                                  large integer calculations. At least
#                                  4 bytes are required.])])


AC_ARG_ENABLE([error-lines],
              AS_HELP_STRING([--enable-error-lines],
                             [If enabled, file and line information is
                              added to the error messages thrown by
                              the library.]),
              [ql_error_lines=$enableval],
              [ql_error_lines=no])
AC_MSG_CHECKING([whether to add file and line information to errors])
if test "$ql_error_lines" = "yes" ; then
   AC_DEFINE([QL_ERROR_LINES],[1],
             [Define this if error messages should include file and line
              information.])
fi
AC_MSG_RESULT([$ql_error_lines])

AC_ARG_ENABLE([error-functions],
              AS_HELP_STRING([--enable-error-functions],
                             [If enabled, current function information
                              is added to the error messages thrown by
                              the library.]),
              [ql_error_functions=$enableval],
              [ql_error_functions=no])
AC_MSG_CHECKING([whether to add function information to errors])
if test "$ql_error_functions" = "yes" ; then
   AC_DEFINE([QL_ERROR_FUNCTIONS],[1],
             [Define this if error messages should include current function
              information.])
fi
AC_MSG_RESULT([$ql_error_functions])

AC_ARG_ENABLE([tracing],
              AS_HELP_STRING([--enable-tracing],
                             [If enabled, tracing messages might be emitted
                              by the library depending on run-time
                              settings. Enabling this option can degrade
                              performance.]),
              [ql_tracing=$enableval],
              [ql_tracing=no])
AC_MSG_CHECKING([whether to enable tracing])
if test "$ql_tracing" = "yes" ; then
   AC_DEFINE([QL_ENABLE_TRACING],[1],
             [Define this if tracing messages should allowed (whether they
              are actually emitted will depend on run-time settings.)])
fi
AC_MSG_RESULT([$ql_tracing])

AC_MSG_CHECKING([whether to enable indexed coupons])
AC_ARG_ENABLE([indexed-coupons],
              AS_HELP_STRING([--enable-indexed-coupons],
                             [If enabled, indexed coupons (see the
                              documentation) are used in floating legs.
                              If disabled (the default), par coupons
                              are used.]),
              [ql_indexed_coupon=$enableval],
              [ql_indexed_coupon=no])
if test "$ql_indexed_coupon" = "yes" ; then
   AC_DEFINE([QL_USE_INDEXED_COUPON],[1],
             [Define this to use indexed coupons instead of par coupons in
              floating legs.])
fi
AC_MSG_RESULT([$ql_indexed_coupon])

AC_MSG_CHECKING([whether to enable extra safety checks])
AC_ARG_ENABLE([extra-safety-checks],
              AS_HELP_STRING([--enable-extra-safety-checks],
                             [If enabled, extra run-time checks are added
                              to a few functions. This can prevent their
                              inlining and degrade performance.]),
              [ql_belt_and_braces=$enableval],
              [ql_belt_and_braces=no])
if test "$ql_belt_and_braces" = "yes" ; then
   AC_DEFINE([QL_EXTRA_SAFETY_CHECKS],[1],
             [Define this if extra safety checks should be performed.
              This can degrade performance.])
fi
AC_MSG_RESULT([$ql_belt_and_braces])

AC_MSG_CHECKING([whether to enable sessions])
AC_ARG_ENABLE([sessions],
              AS_HELP_STRING([--enable-sessions],
                             [If enabled, singletons will return different
                              instances for different sessions. You will
                              have to provide and link with the library
                              a sessionId() function in namespace QuantLib,
                              returning a different session id for each
                              session. This also implies thread-safe
                              Singleton initialization.]),
              [ql_use_sessions=$enableval],
              [ql_use_sessions=no])
if test "$ql_use_sessions" = "yes" ; then
   AC_DEFINE([QL_ENABLE_SESSIONS],[1],
             [Define this if you want to enable sessions.])
fi
AC_MSG_RESULT([$ql_use_sessions])

AC_MSG_CHECKING([whether to enable thread-safe observer pattern])
AC_ARG_ENABLE([thread-safe-observer-pattern],
              AS_HELP_STRING([--enable-thread-safe-observer-pattern],
                             [If enabled, thread-safe version of the 
                              observer pattern will be used. You should 
                              enable it if you want to use QuantLib 
                              via the SWIG layer within the JVM or .NET 
                              eco system or any environment with an 
                              async garbage collector. C++-17 is required
                              if this option is used together with 
                              --enable-std-pointers]),
              [ql_use_tsop=$enableval],
              [ql_use_tsop=no])
AC_MSG_RESULT([$ql_use_tsop])
if test "$ql_use_tsop" = "yes" ; then
   AC_DEFINE([QL_ENABLE_THREAD_SAFE_OBSERVER_PATTERN],[1],
             [Define this if you want to enable 
              thread-safe observer pattern.])
fi

AC_MSG_CHECKING([whether to enable thread-safe singleton initialization])
AC_ARG_ENABLE([thread-safe-singleton-init],
              AS_HELP_STRING([--enable-thread-safe-singleton-init],
                             [If enabled, singleton initialization will
                              be thread-safe. This requires Boost 1.58
                              or later.  Note that --enable-sessions
                              implies thread-safe initialization,
                              whether this is defined or not]),
              [ql_use_safe_singleton_init=$enableval],
              [ql_use_safe_singleton_init=no])
if test "$ql_use_safe_singleton_init" = "yes" ; then
   AC_DEFINE([QL_ENABLE_SINGLETON_THREAD_SAFE_INIT],[1],
             [Define this if you want thread-safe singleton initialization.])
fi
AC_MSG_RESULT([$ql_use_safe_singleton_init])

if test "$ql_use_sessions" = "yes" || test "$ql_use_tsop" = "yes" || test "$ql_use_safe_singleton_init" = "yes"; then
   QL_CHECK_BOOST_VERSION_1_58_OR_HIGHER
   QL_CHECK_BOOST_TEST_THREAD_SIGNALS2_SYSTEM
else
   AC_SUBST([BOOST_THREAD_LIB],[""])
fi

AC_MSG_CHECKING([whether to enable parallel unit test runner])
AC_ARG_ENABLE([parallel-unit-test-runner],
              AS_HELP_STRING([--enable-parallel-unit-test-runner],
                             [If enabled, a parallel unit test runner is used 
                              to execute the C++ test suite. This will reduce 
                              the runtime on multi core CPUs.]),
              [ql_use_parallel_test=$enableval],
              [ql_use_parallel_test=no])
AC_MSG_RESULT([$ql_use_parallel_test])
if test "$ql_use_parallel_test" = "yes" ; then
   AC_DEFINE([QL_ENABLE_PARALLEL_UNIT_TEST_RUNNER],[1],
             [Define this if you want to enable 
              the parallel unit test runner.])
   QL_CHECK_BOOST_VERSION_1_59_OR_HIGHER
   if test "$ql_use_tsop" != "yes" ; then
      QL_CHECK_BOOST_TEST_THREAD_SIGNALS2_SYSTEM
   fi
   QL_CHECK_BOOST_TEST_INTERPROCESS
else
   AC_SUBST([BOOST_INTERPROCESS_LIB],[""])
fi

AC_MSG_CHECKING([whether to install examples])
AC_ARG_ENABLE([examples],
              AS_HELP_STRING([--enable-examples],
                             [If enabled, examples are built and installed
                              when "make" and "make install" are invoked.
                              If disabled (the default) they are built
                              but not installed.]),
              [ql_install_examples=$enableval],
              [ql_install_examples=no])
AM_CONDITIONAL(AUTO_EXAMPLES, test "$ql_install_examples" != "no")
AC_MSG_RESULT([$ql_install_examples])

AC_MSG_CHECKING([whether to install the benchmark])
AC_ARG_ENABLE([benchmark],
              AS_HELP_STRING([--enable-benchmark],
                             [If enabled, the benchmark is built and
                              installed when "make" and "make install"
                              are invoked. If disabled (the default)
                              it is built but not installed.]),
              [ql_install_benchmark=$enableval],
              [ql_install_benchmark=no])
AM_CONDITIONAL(AUTO_BENCHMARK, test "$ql_install_benchmark" != "no")
AC_MSG_RESULT([$ql_install_benchmark])

AC_MSG_CHECKING([whether to use unity build])
AC_ARG_ENABLE([unity-build],
              AS_HELP_STRING([--enable-unity-build],
                             [If enabled, the source files in each directory
                              are collected into one single source file and
                              compiled together. This can speed up the
                              compilation of the library.
                              If disabled (the default) each source file is
                              compiled separately..]),
              [ql_use_unity_build=$enableval],
              [ql_use_unity_build=no])
AM_CONDITIONAL(UNITY_BUILD, test "$ql_use_unity_build" != "no")
AC_MSG_RESULT([$ql_use_unity_build])

AC_MSG_CHECKING([whether to enable intraday date and daycounters])
AC_ARG_ENABLE([intraday],
              AS_HELP_STRING([--enable-intraday],
                             [If enabled, date objects will support an intraday 
                              datetime resolution down to microseconds. 
                              Strickly monotone daycounters (Actual360, 
                              Actual365Fixed and ActualActual) will take 
                              the additional information into account 
                              and allow for accurate intraday pricing. 
                              If disabled (the default) the smallest 
                              resolution of date objects will be a single day.
                              Intraday datetime resolution is experimental.]),
              [ql_use_intraday=$enableval],
              [ql_use_intraday=no])
if test "$ql_use_intraday" = "yes" ; then
   AC_DEFINE([QL_HIGH_RESOLUTION_DATE],[1],
             [Define this if you want to enable high resolution date class.])
fi
AC_MSG_RESULT([$ql_use_intraday])

AC_MSG_CHECKING([whether to enable standard smart pointers])
AC_ARG_ENABLE([std-pointers],
              AS_HELP_STRING([--enable-std-pointers],
                             [If enabled, std::shared_ptr and related
                              classes and functions will be used instead
                              of boost::shared_ptr.
                              If disabled (the default) the Boost facilities
                              are used.
                              Note that std::shared_ptr does not check
                              access and can cause segmentation faults.]),
              [ql_use_std_pointers=$enableval],
              [ql_use_std_pointers=no])
if test "$ql_use_std_pointers" = "yes" ; then
   AC_DEFINE([QL_USE_STD_SHARED_PTR],[1],
             [Define this if you want to use standard smart pointers.])
fi
AC_MSG_RESULT([$ql_use_std_pointers])

AC_MSG_CHECKING([whether to enable std::unique_ptr instead of std::auto_ptr])
AC_ARG_ENABLE([std-unique-ptr],
              AS_HELP_STRING([--enable-std-unique-ptr],
                             [If enabled (the default), std::unique_ptr will
                              be used instead of the deprecated std::auto_ptr.
                              If disabled, std::auto_ptr will still be used.]),
              [ql_use_unique_ptr=$enableval],
              [ql_use_unique_ptr=yes])
if test "$ql_use_unique_ptr" = "yes" ; then
   AC_DEFINE([QL_USE_STD_UNIQUE_PTR],[1],
             [Define this if you want to replace std::auto_ptr with std::unique_ptr.])
fi
AC_MSG_RESULT([$ql_use_unique_ptr])

AC_MSG_CHECKING([whether to enable std::function])
AC_ARG_ENABLE([std-function],
              AS_HELP_STRING([--enable-std-function],
                             [If enabled, std::function and std::bind will be
                              used instead of boost::function and boost::bind.
                              If disabled (the default) the Boost facilities
                              are used.]),
              [ql_use_std_function=$enableval],
              [ql_use_std_function=no])
if test "$ql_use_std_function" = "yes" ; then
   AC_DEFINE([QL_USE_STD_FUNCTION],[1],
             [Define this if you want to use std::function and std::bind.])
fi
AC_MSG_RESULT([$ql_use_std_function])


AC_MSG_CHECKING([whether to enable std::tuple])
AC_ARG_ENABLE([std-tuple],
              AS_HELP_STRING([--enable-std-tuple],
                             [If enabled, std::tuple and related functions
                              will be used instead of boost::tuple.
                              If disabled (the default) the Boost facilities
                              are used.]),
              [ql_use_std_tuple=$enableval],
              [ql_use_std_tuple=no])
if test "$ql_use_std_tuple" = "yes" ; then
   AC_DEFINE([QL_USE_STD_TUPLE],[1],
             [Define this if you want to use std::tuple.])
fi
AC_MSG_RESULT([$ql_use_std_pointers])

AC_MSG_CHECKING([whether to enable available std classes])
AC_ARG_ENABLE([std-classes],
              AS_HELP_STRING([--enable-std-classes],
                             [This is a shortcut for --enable-std-pointers
                              --enable-std-unique-ptr --enable-std-function
                              --enable-std-tuple.
                              If enabled, this supersedes any --disable
                              option passed for the above.]),
              [ql_use_std_classes=$enableval],
              [ql_use_std_classes=no])
if test "$ql_use_std_classes" = "yes" ; then
   AC_DEFINE([QL_USE_STD_SHARED_PTR],[1],
             [Define this if you want to use standard smart pointers.])
   AC_DEFINE([QL_USE_STD_UNIQUE_PTR],[1],
             [Define this if you want to replace std::auto_ptr with std::unique_ptr.])
   AC_DEFINE([QL_USE_STD_FUNCTION],[1],
             [Define this if you want to use std::function and std::bind.])
   AC_DEFINE([QL_USE_STD_TUPLE],[1],
             [Define this if you want to use std::tuple.])
fi
AC_MSG_RESULT([$ql_use_std_classes])


AC_MSG_CHECKING([whether to enable the Disposable class template])
AC_ARG_ENABLE([disposable],
              AS_HELP_STRING([--enable-disposable],
                             [If enabled, the Disposable class template will be used;
                              this should be no longer necessary in C++11
                              and might interfere with compiler optimizations.
                              If disabled (the default) the class will only
                              be an alias for the underlying type.]),
              [ql_use_disposable=$enableval],
              [ql_use_disposable=no])
if test "$ql_use_disposable" = "yes" ; then
   AC_DEFINE([QL_USE_DISPOSABLE],[1],
             [Define this if you want to use the Disposable class template.])
fi
AC_MSG_RESULT([$ql_use_disposable])


# manual configurations for specific hosts
case $host in
  powerpc-apple-darwin*)
    AC_DEFINE([QL_PATCH_DARWIN],[1],
              [Define if running on a Mac OS X machine.])
    ;;
  sparc-sun-solaris*)
    AC_DEFINE([QL_PATCH_SOLARIS],[1],
              [Define if running on a Sun Solaris machine.])
    ;;
esac


# done, output the configured files
AC_CONFIG_FILES([
    Makefile
    ql/Makefile
    ql/cashflows/Makefile
    ql/currencies/Makefile
    ql/experimental/Makefile
    ql/experimental/amortizingbonds/Makefile
    ql/experimental/asian/Makefile
    ql/experimental/averageois/Makefile
    ql/experimental/barrieroption/Makefile
    ql/experimental/basismodels/Makefile
    ql/experimental/callablebonds/Makefile
    ql/experimental/catbonds/Makefile
    ql/experimental/commodities/Makefile
    ql/experimental/coupons/Makefile
    ql/experimental/credit/Makefile
    ql/experimental/exoticoptions/Makefile
    ql/experimental/finitedifferences/Makefile
    ql/experimental/forward/Makefile
    ql/experimental/fx/Makefile
    ql/experimental/inflation/Makefile
    ql/experimental/lattices/Makefile
    ql/experimental/math/Makefile
    ql/experimental/mcbasket/Makefile
    ql/experimental/models/Makefile
    ql/experimental/processes/Makefile
    ql/experimental/risk/Makefile
    ql/experimental/shortrate/Makefile
    ql/experimental/swaptions/Makefile
    ql/experimental/termstructures/Makefile
    ql/experimental/variancegamma/Makefile
    ql/experimental/varianceoption/Makefile
    ql/experimental/volatility/Makefile
    ql/indexes/Makefile
    ql/indexes/ibor/Makefile
    ql/indexes/inflation/Makefile
    ql/indexes/swap/Makefile
    ql/instruments/Makefile
    ql/instruments/bonds/Makefile
    ql/legacy/Makefile
    ql/legacy/libormarketmodels/Makefile
    ql/math/Makefile
    ql/math/copulas/Makefile
    ql/math/distributions/Makefile
    ql/math/integrals/Makefile
    ql/math/interpolations/Makefile
    ql/math/matrixutilities/Makefile
    ql/math/ode/Makefile
    ql/math/optimization/Makefile
    ql/math/randomnumbers/Makefile
    ql/math/solvers1d/Makefile
    ql/math/statistics/Makefile
    ql/methods/Makefile
    ql/methods/finitedifferences/Makefile    
    ql/methods/finitedifferences/meshers/Makefile
    ql/methods/finitedifferences/operators/Makefile
    ql/methods/finitedifferences/schemes/Makefile
    ql/methods/finitedifferences/solvers/Makefile
    ql/methods/finitedifferences/stepconditions/Makefile
    ql/methods/finitedifferences/utilities/Makefile
    ql/methods/lattices/Makefile
    ql/methods/montecarlo/Makefile
    ql/models/Makefile
    ql/models/equity/Makefile
    ql/models/marketmodels/Makefile
    ql/models/marketmodels/browniangenerators/Makefile
    ql/models/marketmodels/callability/Makefile
    ql/models/marketmodels/correlations/Makefile
    ql/models/marketmodels/curvestates/Makefile
    ql/models/marketmodels/driftcomputation/Makefile
    ql/models/marketmodels/evolvers/Makefile
    ql/models/marketmodels/evolvers/volprocesses/Makefile
    ql/models/marketmodels/models/Makefile
    ql/models/marketmodels/pathwisegreeks/Makefile
    ql/models/marketmodels/products/Makefile
    ql/models/marketmodels/products/onestep/Makefile
    ql/models/marketmodels/products/multistep/Makefile
    ql/models/marketmodels/products/pathwise/Makefile
    ql/models/shortrate/Makefile
    ql/models/shortrate/calibrationhelpers/Makefile
    ql/models/shortrate/onefactormodels/Makefile
    ql/models/shortrate/twofactormodels/Makefile
    ql/models/volatility/Makefile
    ql/patterns/Makefile
    ql/pricingengines/Makefile
    ql/pricingengines/asian/Makefile
    ql/pricingengines/barrier/Makefile
    ql/pricingengines/basket/Makefile
    ql/pricingengines/bond/Makefile
    ql/pricingengines/capfloor/Makefile
    ql/pricingengines/cliquet/Makefile
    ql/pricingengines/credit/Makefile
    ql/pricingengines/forward/Makefile
    ql/pricingengines/inflation/Makefile
    ql/pricingengines/lookback/Makefile
    ql/pricingengines/quanto/Makefile
    ql/pricingengines/swap/Makefile
    ql/pricingengines/swaption/Makefile
    ql/pricingengines/vanilla/Makefile
    ql/processes/Makefile
    ql/quotes/Makefile
    ql/termstructures/Makefile
    ql/termstructures/credit/Makefile
    ql/termstructures/inflation/Makefile
    ql/termstructures/volatility/Makefile
    ql/termstructures/volatility/equityfx/Makefile
    ql/termstructures/volatility/capfloor/Makefile
    ql/termstructures/volatility/inflation/Makefile
    ql/termstructures/volatility/optionlet/Makefile
    ql/termstructures/volatility/swaption/Makefile
    ql/termstructures/yield/Makefile
    ql/time/Makefile
    ql/time/calendars/Makefile
    ql/time/daycounters/Makefile
    ql/utilities/Makefile
    man/Makefile
    m4/Makefile
    Docs/Makefile
    Examples/Makefile
    test-suite/Makefile])
AC_CONFIG_FILES([quantlib-config],[chmod +x quantlib-config])
AC_CONFIG_FILES([QuantLib.spec])
AC_CONFIG_FILES([quantlib.pc])
AC_OUTPUT

