import os
import sys
import excons
import excons.tools.dl as dl

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
   if tbb_static:
      env.Append(CPPDEFINES=["TBB_STATIC"])
   env.Append(CPPPATH=[excons.OutputBaseDirectory() + "/include"])
   excons.Link(env, TBBPath(), static=tbb_static, force=True, silent=True)



cmake_opts = {"TBB_BUILD_SHARED": (0 if tbb_static else 1),
              "TBB_BUILD_STATIC": (1 if tbb_static else 0),
              "TBB_BUILD_TBBMALLOC": 1,
              "TBB_BUILD_TBBMALLOC_PROXY": 1,
              "TBB_BUILD_TESTS": excons.GetArgument("tbb-tests", 0, int)}

cmake_srcs = excons.CollectFiles(["src/tbb", "src/tbbmalloc", "src/tbbproxy", "src/rml", "src/perf"],
                                 patterns=["*.cpp", "*.def", "*.asm"], recursive=True)

tbb = {"name": "tbb",
       "type": "cmake",
       "cmake-opts": cmake_opts,
       "cmake-cfgs": ["CMakeLists.txt"],
       "cmake-srcs": cmake_srcs,
       "cmake-outputs": [TBBPath(), TBBMallocPath(), TBBProxyPath()] +
                        map(lambda x: "include/tbb/%s" % os.path.basename(x), excons.glob("include/tbb/*.h"))}

excons.AddHelpOptions(tbb="""TBB OPTIONS
  tbb-static=0|1 : Toggle between static and shared version of the libraries. [1]
  tbb-tests=0|1  : Compile TBB tests. [0]""")

excons.DeclareTargets(env, [tbb])

Export("TBBName TBBPath RequireTBB TBBMallocName TBBMallocPath TBBProxyName TBBProxyPath")

