# RPM Build Instructions for Sticky

## Prerequisites
```bash
# Install RPM build tools
sudo dnf install rpm-build rpmdevtools

# Setup RPM build environment
rpmdev-setuptree
```

## Method 1: Build Directly from Current Directory
```bash
# Navigate to project root
cd /home/ben/Code/sticky

# Build RPM directly using current directory structure
rpmbuild -bb fedora/sticky.spec --define "_sourcedir $(pwd)" --define "_builddir $(pwd)"
```

## Method 2: Traditional RPM Build (Alternative)
```bash
# Navigate to project root
cd /home/ben/Code/sticky

# Create source tarball
tar --exclude='.git' --exclude='*.tar.gz' -czf sticky-1.26.tar.gz .

# Copy to RPM SOURCES directory
cp sticky-1.26.tar.gz ~/rpmbuild/SOURCES/

# Build RPM
rpmbuild -bb fedora/sticky.spec
```

## Method 3: Build RPM in Project Directory (Local Build)
```bash
# Create local RPM build structure in project directory
mkdir -p /home/ben/Code/sticky/rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}

# Build RPM with output directed to project directory
rpmbuild -bb --define "_topdir /home/ben/Code/sticky/rpmbuild" fedora/sticky.spec

# Find the built RPM in the local directory
ls /home/ben/Code/sticky/rpmbuild/RPMS/noarch/
```

## Install Build Dependencies (if build fails)
```bash
# Install required build dependencies
sudo dnf builddep fedora/sticky.spec
```

## Find and Install Built RPM
```bash
# Check build results
ls ~/rpmbuild/RPMS/noarch/

# Install the built package
sudo dnf install ~/rpmbuild/RPMS/noarch/sticky-1.26-0.fc*.noarch.rpm
```

## Troubleshooting
- If build fails due to missing dependencies, run the builddep command above
- Ensure version in spec file matches your source version
- Check that all files listed in %files section actually exist in your build