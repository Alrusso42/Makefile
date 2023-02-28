#==--------------------------------------==#
# *                                      * #
#           LISTING FILES & DIR            #
# *                                      * #
#==--------------------------------------==#

#### ===> Méthode 1 ####
#VERSION_FILE  := version.txt
#VERSION       := $(shell cat $(VERSION_FILE))
#VERSION_NEXT  := $(shell echo $$(($(VERSION)+1)))
#CURRENT_FOLDER = $(shell basename "$(shell pwd)")
#NAME           = $(CURRENT_FOLDER)
#DIR_NAME      := $(shell basename "$(CURDIR)")
#SRC_FILES      = Nom des fichiers sources sans les extensions (.cpp/ & .c/)
#SRC_DIR        = Nom choisi pour le dossier où les fichiers sources seront déplacés
#INC_FILES      = Nom des fichiers includes sans les extensions (.hpp/ & .h/)
#INC_DIR        = Nom choisi pour le dossier où les fichiers includes seront déplacés
#OBJ_DIR        = Nom choisi pour le dossier où les fichiers objets seront déplacés
#ARCHIVE_DIR   := Nom choisi pour le dossier où les fichiers .zip seront déplacés
#SRC_OUT        = $(foreach f, $(SRC_FILES), $(wildcard $(f)*.c*))
#SRC_IN         = $(foreach f, $(SRC_FILES), $(wildcard $(SRC_DIR)/$(f)*.c*))
#INC_OUT        = $(foreach f, $(INC_FILES), $(wildcard $(f)*.h*))
#INC_IN         = $(foreach f, $(INC_FILES), $(wildcard $(INC_DIR)/$(f)*.h*))
#LIBS           = Chemin pour accéder aux bibliothèques
#OBJ_OUT        = $(filter $(wildcard *.c*), $(SRC_OUT))
#OBJ_IN         = $(filter $(wildcard $(SRC_DIR)/*.c*), $(SRC_IN))
#OBJ            = $(patsubst %.cpp, %.o, \
#                 $(patsubst %.c, %.o, \
#                 $(OBJ_OUT) $(OBJ_IN)))


#### ===> Méthode 2 ####
#VERSION_FILE  := version.txt
#VERSION       := $(shell cat $(VERSION_FILE))
#VERSION_NEXT  := $(shell echo $$(($(VERSION)+1)))
CURRENT_FOLDER = $(shell basename "$(shell pwd)")
NAME           = $(CURRENT_FOLDER)
DIR_NAME      := $(shell basename "$(CURDIR)")
SRC_DIR        = src
INC_DIR        = inc
OBJ_DIR        = bin
ARCHIVE_DIR   := archive
SRC_IN         = $(wildcard $(SRC_DIR)/*.c*)
SRC_OUT        = $(wildcard *.c*)
INC_IN         = $(wildcard $(INC_DIR)/*.h*)
INC_OUT        = $(wildcard *.h*)
#LIBS           = Chemin pour accéder aux bibliothèques
OBJ_OUT        = $(filter $(wildcard *.c*), $(SRC_OUT))
OBJ_IN         = $(filter $(wildcard $(SRC_DIR)/*.c*), $(SRC_IN))
OBJ            = $(patsubst %.cpp, %.o, \
                 $(patsubst %.c, %.o, \
                 $(OBJ_OUT) $(OBJ_IN)))

#==--------------------------------------==#
# *                                      * #
#           COMPILATION ADAPTATIVE         #
#           		&          		       #
#          UTILITAIRES DE COMPILATION      #
# *                                      * #
#==--------------------------------------==#

ifeq ($(SRC_OUT .cpp), )
	CC = c++
else
	CC = gcc
endif

FLAGS    = -Wall -Werror -Wextra -g3
CPPFLAGS = -Wall -Werror -Wextra -g3 -std=c++98
DANGER   = -fsanitize=address
NOERR    = || true
STOP     = && false
SILENT   = > /dev/null 2>&1 | true
CONTINUE = $(SILENT) $(NOERR)

#==--------------------------------------==#
# *                                      * #
#           	  RÈGLES         		   #
# *                                      * #
#==--------------------------------------==#

all: check $(NAME)


$(OBJ_DIR)/%.o: $(SRC_DIR)/%.cpp
	@c++ $(CPPFLAGS) -c $< -o $@

$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	@gcc $(FLAGS) -c $< -o $@

.cpp.o:
	@c++ $(CPPFLAGS) -c $< -o $@
	@printf "$(CMP_WORK_CT)"
	@$(eval CMP_COUNT = $(shell expr $(CMP_COUNT) + 1))

.c.o:
	@gcc $(FLAGS) -c $< -o $@
	@printf "$(CMP_WORK_CT)"
	@$(eval CMP_COUNT = $(shell expr $(CMP_COUNT) + 1))

$(NAME): $(OBJ)
	@$(CC) $(LIBS) $(DANGER) $(OBJ) -o $@
	@$(MAKE) pack

check:
	@if [ -d $(OBJ_DIR) ] && [ -d $(SRC_DIR) ] && [ -d $(INC_DIR) ]; then \
		echo "Nothing to do"; \
		exit 1 >/dev/null 2>&1; \
	fi

pack:
	@mkdir -p $(SRC_DIR) $(INC_DIR) $(OBJ_DIR)
	@mv $(SRC_OUT) $(SRC_DIR) $(SILENT)
	@mv $(INC_OUT) $(INC_DIR) $(SILENT)
	@mv $(OBJ) $(OBJ_DIR) $(SILENT)
	@printf "$(PKG_SUCCESS)"

unpack:
	@if [ -d ./src ]; \
	then \
	    mv $(SRC_IN) . && rm -R ./src && printf "$(CLR_PACKAGE)"; \
	else \
	    printf "$(CLR_PKGFAIL)"; \
	fi
	@if [ -d ./inc ]; \
	then \
	    mv $(INC_IN) . && rm -R ./inc && printf "$(CLR_PACKAGE)"; \
	else \
	    printf "$(CLR_PKGFAIL)"; \
	fi
	@if [ -d ./bin ]; \
	then \
	    mv $(OBJ_DIR)/$(OBJ) . && rm -R ./bin && printf "$(CLR_PACKAGE)"; \
	else \
	    printf "$(CLR_PACKAGE)"; \
	fi

clean:
	@if [ -d ./bin ]; \
	then \
	    rm -Rf ./bin && printf "$(CLR_SUCCESS)"; \
	else \
	    printf "$(CLR_FAILURE)"; \
	fi

fclean: clean unpack
	@if [ -f $(NAME) ]; \
		then rm -Rf $(NAME) \
		&& printf "$(CLR_EXECUTE)"; \
		else printf "$(CLR_EXEFAIL)"; \
	fi

re: fclean
	@$(MAKE) all

#version:
#	@echo "$(VERSION)" > version.txt

#zip: version
#	@mkdir -p $(ARCHIVE_DIR)
#	@zip -q $(ARCHIVE_DIR)/$(DIR_NAME)_archive_v$(VERSION).zip * -x "$(ARCHIVE_DIR)/*"
#	@VERSION=$$(($(shell cat version.txt) + 1));
#	@echo "$(VERSION)" > $(VERSION_FILE)
#	@printf "$(CMP_WORK_ZP)"
#	@$(eval CMP_COUNT_ZP = $(shell expr $(CMP_COUNT_ZP) + 1))
#	@printf "$(ZIP_SUCCESS)";
# Définition du nom de l'archive et de sa version
# Récupération du numéro de version actuel depuis le fichier "version.txt"
# Vérification si la cible "zip" est appelée
# Vérification si la cible "zip" est appelée
ifeq (zip,$(filter zip,$(MAKECMDGOALS)))
	# Récupération du numéro de version actuel depuis le fichier "version.txt"
VERSION := $(shell cat .version 2>/dev/null)

	# Incrémentation du numéro de version et écriture dans le fichier ".version"
NEW_VERSION := $(shell echo $$(($(VERSION) + 1)))
$(shell echo $(NEW_VERSION) > .version)

	# Définition du nom de l'archive avec le nouveau numéro de version
ARCHIVE_NAME = $(notdir $(CURDIR))_archive_v$(NEW_VERSION).zip
else
	# Récupération du numéro de version actuel depuis le fichier ".version"
VERSION := $(shell cat .version 2>/dev/null)

	# Définition du nom de l'archive avec le numéro de version actuel
ARCHIVE_NAME = $(notdir $(CURDIR))_archive_v$(VERSION).zip
endif

# Définition de la cible "zip"
zip: | archive
	@echo "Création de l'archive $(ARCHIVE_NAME)"
	@zip -q -r archive/$(ARCHIVE_NAME) .

# Création du dossier "archive" s'il n'existe pas déjà
archive:
	@mkdir -p archive

.PHONY: zip archive



czip:
	@if [ -d "archive" ]; then rm -rf archive; fi
	@rm -f .version
	@printf "$(CLR_ZIP)";

#==--------------------------------------==#
# *                                      * #
#           MESSAGES & AFFICHAGE           #
# *                                      * #
#==--------------------------------------==#

#####   AFFICHAGE   #####

ESC			= 
ICO_PROCESS	= ƒ
ICO_SUCCESS	= √
ICO_FAILURE	= ø
NEWLINE	= \n
BREAK	= \r
RED			= $(ESC)[0;31m
GRN			= $(ESC)[0;32m
YLW			= $(ESC)[0;33m
BLU			= $(ESC)[0;34m
DRK			= $(ESC)[0;2m
NUL			= $(ESC)[0m
END			= $(ESC)[0m$(NEWLINE)
BACK		= $(ESC)[2K$(BREAK)


#####   MESSAGE PRÉFAIT   #####

MSG_WORK	= $(BACK)$(YLW)$(ICO_PROCESS)
MSG_GOOD	= $(BACK)$(GRN)$(ICO_SUCCESS)
MSG_ERROR	= $(BACK)$(RED)$(ICO_FAILURE)
MSG_NRET    = $(NUL)

#####   MESSAGE DE COMPILATION  #####

CMP_NEEDING	= $(MSG_WORK) Compiling dependencies ... $(NUL)
CMP_WORKING	= $(MSG_WORK) Compiling $@ ... $(NUL)
CMP_SUCCESS	= $(MSG_GOOD) The programm $(NAME) has been compiled successfully! $(END)
CMP_FAILURE	= $(MSG_ERROR) The programm $(NAME) failed to compile! $(END)
PKG_SUCCESS = $(MSG_GOOD) The package is ready for export! $(END)
ZIP_SUCCESS = $(MSG_GOOD) The zip files is ready for export! $(END)

#####   MESSAGE DE NETTOYAGE   #####

CLR_NEEDING	= $(MSG_WORK) Cleaning dependencies ... $(NUL)
CLR_WORKING	= $(MSG_WORK) Cleaning files ... $(NUL)
CLR_SUCCESS	= $(MSG_GOOD) Objects has been removed! $(END)
CLR_FAILURE	= $(MSG_ERROR) Objects couldn't be removed! $(END)
CLR_EXECUTE	= $(MSG_GOOD) Executable has been removed! $(END)
CLR_EXEFAIL	= $(MSG_ERROR) Executable couldn't be removed! $(END)
CLR_PACKAGE	= $(MSG_GOOD) Package has been removed! $(END)
CLR_PKGFAIL	= $(MSG_ERROR) Package couldn't be removed! $(END)
CLR_DEPENDS	= $(MSG_GOOD) Dependencies has been removed! $(END)
CLR_DEPFAIL	= $(MSG_ERROR) Dependencies couldn't be removed! $(END)
CLR_ZIP	= $(MSG_GOOD) Zip file has been removed! $(END)

#####   BARRE DE PROGRESSION COMPILATION   #####

CMP_WORK_CT	= $(MSG_WORK) [$(CMP_COUNT) / $(CMP_TOTAL)] Compiling $@ ... $(MSG_NRET)
CMP_TOTAL	= $(shell awk -F' ' '{printf NF}' <<< "$(SRC)")
CMP_COUNT	= 0

#####   BARRE DE PROGRESSION COMPRESSION  #####

CMP_WORK_ZP	= $(MSG_WORK) [$(CMP_COUNT_ZP) / $(CMP_TOTAL_ZP)] Compressing $@ ... $(MSG_NRET)
CMP_TOTAL_ZP	= $(shell awk -F' ' '{printf NF}' <<< "$(SRC)")
CMP_COUNT_ZP	= 0

#==--------------------------------------==#
# *                                      * #
#           DÉFINITION DE CIBLES           #
# *                                      * #
#==--------------------------------------==#

.DEFAULT_GOAL = all
.PHONY: all clean fclean pack unpack zip czip re archive
