- 👋 Hi, I’m @Sachin91rr
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---import cv2

# Read the image file
image = cv2.imread('path/to/image.jpg')



# Display some information about the image
print(image.shape)
print(image.dtype)

# Perform image processing operations here
# ...

# Release the image file
cv2.release()
Sachin91rr/Sachin91rr is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
_get_zlink () {
    local regex
    regex='(https?)://github.com/.+/.+'
    if [[ $UPSTREAM_REPO == "pro" ]]
    then
        echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9UL2FyY2hpdmUvbWFzdGVyLnppcA==" | base64 -d
    elif [[ $UPSTREAM_REPO == "multi" ]]
    then
        echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9UL2FyY2hpdmUvbWFzdGVyLnppcA==" | base64 -d
    elif [[ $UPSTREAM_REPO =~ $regex ]]
    then
        if [[ $UPSTREAM_REPO_BRANCH ]]
        then
            echo "${UPSTREAM_REPO}/archive/${UPSTREAM_REPO_BRANCH}.zip"
        else
            echo "${UPSTREAM_REPO}/archive/master.zip"
        fi
    else
        echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9UL2FyY2hpdmUvbWFzdGVyLnppcA==" | base64 -d
    fi
}

_get_repolink () {
    local regex
    local rlink
    regex='(https?)://github.com/.+/.+'
    if [[ $UPSTREAM_REPO == "pro" ]]
    then
        rlink=`echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9U" | base64 -d`
    elif [[ $UPSTREAM_REPO == "multi" ]]
    then
        rlink=`echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9U" | base64 -d`
    elif [[ $UPSTREAM_REPO =~ $regex ]]
    then
        rlink=`echo "${UPSTREAM_REPO}"`
    else
        rlink=`echo "aHR0cHM6Ly9naXRodWIuY29tL1RFQ0hOT0JPVC1PUC9URUNITk9VU0VSQk9U" | base64 -d`
    fi
    echo "$rlink"
}


_run_python_code() {
    python3${pVer%.*} -c "$1"
}

_run_technopackgit() {
    $(_run_python_code 'from git import Repo
import sys
OFFICIAL_UPSTREAM_REPO = "https://github.com/TECHNOBOT-OP/TECHNOBOT"
ACTIVE_BRANCH_NAME = "master"
repo = Repo.init()
origin = repo.create_remote("temponame", OFFICIAL_UPSTREAM_REPO)
origin.fetch()
repo.create_head(ACTIVE_BRANCH_NAME, origin.refs[ACTIVE_BRANCH_NAME])
repo.heads[ACTIVE_BRANCH_NAME].checkout(True) ')
}

_run_technogit() {
    local repolink=$(_get_repolink)
    $(_run_python_code 'from git import Repo
import sys
OFFICIAL_UPSTREAM_REPO="'$repolink'"
ACTIVE_BRANCH_NAME = "'$UPSTREAM_REPO_BRANCH'" or "master"
repo = Repo.init()
origin = repo.create_remote("temponame", OFFICIAL_UPSTREAM_REPO)
origin.fetch()
repo.create_head(ACTIVE_BRANCH_NAME, origin.refs[ACTIVE_BRANCH_NAME])
repo.heads[ACTIVE_BRANCH_NAME].checkout(True) ')
}

_start_bot () {
    local zippath
    zippath="TECHNOBOT.zip"
    echo "  Downloading source code ..."
    wget -q $(_get_zlink) -O "$zippath"
    echo "  Unpacking Data ..."
    TECHNOPATH=$(zipinfo -1 "$zippath" | grep -v "/.");
    unzip -qq "$zippath"
    echo "Done"
    echo "  Cleaning ..."
    rm -rf "$zippath"
    _run_technopackgit
    cd $TECHNOPATH
    _run_technogit
    python3 ../setup/updater.py ../requirements.txt requirements.txt
    chmod -R 755 bin
    echo "    Starting TechnoUserBot    "
    python3 -m Technobot
}

_start_bot
