U	= poly_dec poly_lib datadec ioutils effut elemutils rot xvutils getsur numutils surveysub 
#U	= poly_lib effut getsur numutils surveysub
OBJU	= $(addsuffix .o,${U})
SRCU	= $(addsuffix .f95,${U})
FPPU	= $(addsuffix .fpp,${U})
MODU	= $(addsuffix .mod,${U})

M   = modelutils GiMeObj
OBJM	= $(addsuffix .o,${M})
SRCM	= $(addsuffix .f95,${M})
FPPM	= $(addsuffix .fpp,${M})
MODM	= $(addsuffix .mod,${M})


A	= poly_dec datadec ioutils elemutils rot
OBJA	= $(addsuffix .o,${A})
SRCA	= $(addsuffix .f95,${A})
FPPA	= $(addsuffix .fpp,${A})
MODA	= $(addsuffix .mod,${A})


SSIM	= SurveySubsF95
GOBJ	= GiMeObjF95

FC	= gfortran
FPP	= gfortran -E -x f95-cpp-input -fPIC -DPYTHON

FFLAGS	= -O3 -fPIC -x f95-cpp-input
FFLAGP	= -O3
CFLAGS  = 
CPPFLAGS = 

SUBDIRS := example
TOPTARGETS := clean test
GIMEOBJ ?= InnerBeltModel

$(TOPTARGETS): $(SUBDIRS)

.PHONY: $(SUBDIRS) 

$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

all:	Driver $(SUBDIRS)

%.fpp :	%.f95
	$(FPP) $< -o $@

%.o : %.f95
	$(FC) $(FFLAGS) -c $<

.PHONY: clean

clean:
	rm -f *~ *.o *.mod *.fpp f90wrap*f90 $(SSIM).py $(GOBJ).py _$(SSIM)*.so _$(GOBJ)*.so SimulDetect.dat SimulTrack.dat
	rm -f *~ $(SSIM).py $(GOBJ).py $(IOBJ).py $(ROBJ).py _$(SSIM)*.so _$(GOBJ)*.so _$(IOBJ)*.so _$(ROBJ)*.so
	rm -f *.log LOG .f2py_f2cmap Driver test_read_pointings  GeMeObj.f95

.PHONY: link

link:
	\rm -f GiMeObj.f95
	\rm -f GiMeObj.o
	ln -s $(GIMEOBJ).f95 GiMeObj.f95

Driver: link Driver.f95 $(OBJU) ${SRCM} Makefile
	$(FC) $(FFLAGP) -o Driver $(OBJU) ${SRCM} Driver.f95
	rm GiMeObj.f95

Modules: _$(SSIM).so _$(GOBJ).so Makefile
	echo "Modules have been built"

_$(SSIM).so: $(FPPU) $(OBJU) Makefile
	\rm -f _$(SSIM).so
	f90wrap -k kind_map -m $(SSIM) $(FPPU) 1>f90wrap_SurveySubs.log 2>&1
	f2py-f90wrap --fcompiler=$(FC) -c -m _$(SSIM) $(OBJU) f90wrap_*.f90 1>f2py_SurveySubs.log 2>&1
	\rm f90wrap_*.f90
	mv _$(SSIM)*.so _$(SSIM).so

_$(GOBJ).so: link $(FPPM) $(OBJM) $(FPPA) $(OBJA) _$(SSIM).so $(GIMEOBJ).f95 Makefile
	\rm -f _$(GOBJ).so
	f90wrap -k kind_map -m $(GOBJ) $(FPPM) $(FPPA) 1>f90wrap_GiMeObj.log 2>&1
	f2py-f90wrap --fcompiler=$(FC) -c -m _$(GOBJ) $(OBJA) $(OBJM) f90wrap_*.f90 1>f2py_GiMeObj.log 2>&1
	\rm GiMeObj.f95
	\rm f90wrap_*.f90
	mv _$(GOBJ)*.so _$(GOBJ).so
