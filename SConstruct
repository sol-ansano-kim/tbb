import os
import sys
import excons

env = excons.MakeBaseEnv()


tbb_static = (excons.GetArgument("tbb-static", 1, int) != 0)

def TBBName():
   return "tbb" + ("_static" if tbb_static else "")

def TBBPath():
   name = TBBName()
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + (".a" if tbb_static else excons.SharedLibraryLinkExt())
   return excons.OutputBaseDirectory() + "/lib/" + libname 

def TBBMallocName():
   return "tbbmalloc" + ("_static" if tbb_static else "")

def TBBMallocPath():
   name = TBBMallocName()
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + (".a" if tbb_static else excons.SharedLibraryLinkExt())
   return excons.OutputBaseDirectory() + "/lib/" + libname 

def TBBProxyName():
   return "tbbmalloc_proxy" + ("_static" if tbb_static else "")

def TBBProxyPath():
   name = TBBProxyName()
   if sys.platform == "win32":
      libname = name + ".lib"
   else:
      libname = "lib" + name + (".a" if tbb_static else excons.SharedLibraryLinkExt())
   return excons.OutputBaseDirectory() + "/lib/" + libname 

def RequireTBB(env):
   if static:
      # HUM?
      pass
   env.Append(CPPPATH=[excons.OutputBaseDirectory() + "/include"])
   env.Append(LIBS=[TBBPath()])




cmake_opts = {"TBB_BUILD_SHARED": (0 if tbb_static else 1),
              "TBB_BUILD_STATIC": (1 if tbb_static else 0),
              "TBB_BUILD_TBBMALLOC": 1,
              "TBB_BUILD_TBBMALLOC_PROXY": 1,
              "TBB_BUILD_TESTS": 0}

cmake_srcs = excons.CollectFiles(["src/tbb", "src/tbbmalloc", "src/tbbproxy", "src/rml", "src/perf"],
                                 patterns=["*.cpp", "*.def", "*.asm"], recursive=True)

tbb = {"name": "tbb",
       "type": "cmake",
       "cmake-opts": cmake_opts,
       "cmake-cfgs": ["CMakeLists.txt"],
       "cmake-srcs": cmake_srcs,
       "cmake-outputs": [TBBPath(), TBBMallocPath(), TBBProxyPath()] +
                        map(lambda x: "include/tbb/%s" % os.path.basename(x), excons.glob("include/tbb/*.h"))}

excons.DeclareTargets(env, [tbb])

Export("TBBName TBBPath RequireTBB TBBMallocName TBBMallocPath TBBProxyName TBBProxyPath")

