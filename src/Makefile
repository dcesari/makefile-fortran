# A simple hand-made makefile for fortran 90 applications
# including most of the usual dependency cases

# ======================================================================
# Let's start with the declarations
# ======================================================================

# The compiler
FC = gfortran
# flags for debugging or for maximum performance, comment as necessary
FCFLAGS = -g -fcheck-bounds
FCFLAGS = -O2
# flags forall (look for system .mod files required in gfortran)
FCFLAGS += -I/usr/include

# libraries needed for linking
#LDFLAGS = -li_need_this_lib

# List of executables to be built
PROGRAMS = exe1 exe2 exe3 exe4

# "make" builds all
all: $(PROGRAMS)

# ======================================================================
# Here come the most interesting part: the rules for exe1, exe2, exe3
# and exe4, modify to suit your needs
# ======================================================================

# exe1 is simply built by exe1.f90 through exe1.o, nothing to specify
# for it

# exe2.o, besides depending on exe2.f90, depends also on exe2.incf
# through include, if exe2.incf is newer than exe2.o, exe2.o has to be
# rebuilt; it not necessary to confirm the dependency of exe2.o on
# exe2.f90, the specific dependency is just added to the general rule
exe2.o: exe2.incf

# exe3 requires to link in, not just exe3.o, but also aux.o, which
# contains a subroutine called from within exe3.f90; moreover exe3 has
# to be rebuilt if aux.o is newer; both are accomplished by adding an
# explicit dependency; as before, no need to confirm that exe3 depends
# on exe3.o as well
exe3: aux.o

# exe4.f90 USEs a module from mod.f90, there is no standard naming
# convention for modules in f90, but the dependency can be built on
# the object files (when mod.o is generated, I am sure that
# any_module_inside_mod.mod has been just generated as well); mod.o
# has also to be linked in, so the dependency is added also for the
# executable
exe4.o: mod.o
exe4: mod.o

# ======================================================================
# And now the general rules, these should not require modification
# ======================================================================

# General rule for building exe from exe.o; $^ is used in order to
# list additional object files on which the executable depends
%: %.o
	$(FC) $(LDFLAGS) -o $@ $^

# General rule for building exe.o from exe.f90 or exe.F90; $< is used
# in order to list only the first prerequisite (the source file) and
# not the additional prerequisites such as module or include files
%.o: %.f90
	$(FC) -c $(FCFLAGS) $<

%.o: %.F90
	$(FC) -c $(FCFLAGS) $<

# Utility targets
.PHONY: clean veryclean

clean:
	rm -f *.o *.mod .MOD

veryclean: clean
	rm -f *~ $(PROGRAMS)

