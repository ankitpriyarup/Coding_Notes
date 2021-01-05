# Geometry Theory

```c++
const double EPS = 1e-7;
struct Point
{
    double x, y, z;
    Point() : x(0), y(0), z(0) { }
    Point(double _x, double _y) : x(_x), y(_y), z(0) { }
    Point(double _x, double _y, double _z) : x(_x), y(_y), z(_z) { }
    Point operator+(const Point &p) { return Point(x+p.x, y+p.y, z+p.z); }
    Point operator-(const Point &p) { return Point(x-p.x, y-p.y, z-p.z); }
    Point operator*(double c) { return Point(x*c, y*c, z*c); }
    Point operator/(double c) { return Point(x/c, y/c, z/c); }
};
typedef Point Vector;
typedef pair<Point, Point> Line;
typedef vector<Point> Polygon;
struct Plane { double a, b, c, d; };  // ax + by + cz = d form
double crossProduct(const Vector &a, const Vector &b) { return a.x * b.y - a.y * b.x; }
double dotProduct(const Vector &a, const Vector &b) { return a.x * b.x + a.y * b.y; }
double sqrDist(Point p, Point q) { return dotProduct(p-q, p-q); }

// internal used functions
vector< pair<double, double> > vec;
double length(const Vector &a) { return sqrt(dotProduct(a, a)); }
inline int dcmp(double x) { return (x > 1e-10) - (x < -(1e-10)); }
inline void add(double x, double flag) { vec.push_back(make_pair(x, -flag)); }
double get(Point &p, Vector &v, Point &q, Vector &w) { return crossProduct(w, p-q) / crossProduct(v, w); }
void getSegmentIntersection(Point p, Vector v, Point q, Vector w)
{
    if (!dcmp(crossProduct(v, w)))
    {
        if (!dcmp(crossProduct(v, q-p)))
        {
            double l = dotProduct(v, q-p) / dotProduct(v, v);
            double r = dotProduct(v, q+w-p) / dotProduct(v, v);
            if (l > r) swap(l, r);
            add(l, +1); add(r, -1);
        }
    }
    else
    {
        static int num = 0;
        double temp = get(q, w, p, v);
        if (dcmp(temp) >= 0 && dcmp(temp-1) <= 0)
        {
            double rate = 1;
            if (dcmp(temp) == 0 || dcmp(temp-1) == 0) rate = .5;
            temp = get(p, v, q, w);
            add(temp, dcmp(crossProduct(w, v)) > 0 ? +rate : -rate);
        }
    }
}

// Helper functions
Point rotateCounterClockwise(Point pt, double radians) { return Point(pt.x*cos(radians) - pt.y*sin(radians), pt.x*sin(radians) + pt.y*cos(radians)); }
Point rotateCounterClockwise(Point pt, double radians, Point ref) { return rotateCounterClockwise(pt-ref, radians)+ref; }
Point rotateClockwise(Point pt, double radians) { return rotateCounterClockwise(pt, -radians); }
Point rotateClockwise(Point pt, double radians, Point ref) { return rotateCounterClockwise(pt, -radians, ref); }
double pointPlaneDistance(Point a, Plane b) { return fabs(b.a*a.x + b.b*a.y + b.c*a.z - b.d) / sqrt(b.a*b.a + b.b*b.b + b.c*b.c); }
Point projectPointOnLine(Line a, Point b) { return a.F + (a.S-a.F) * dotProduct(b-a.F, a.S-a.F)/dotProduct(a.S-a.F, a.S-a.F); }
Point projectPointOnLineSegment(Line a, Point b)
{
    double r = dotProduct(a.S-a.F, a.S-a.F);
    if (fabs(r) < EPS) return a.F;
    r = dotProduct(b-a.F, a.S-a.F)/r;
    if (r < 0) return a.F;
    if (r > 1) return a.S;
    return a.F + (a.S-a.F)*r;
}
bool linesParalle(Line a, Line b) { return (fabs(crossProduct(a.S-a.F, b.F-b.S)) < EPS); }
bool linesCollinear(Line a, Line b) { return (linesParalle(a, b) && (fabs(crossProduct(a.F-b.F, b.S-a.S)) < EPS)); }
bool linesIntersect(Line a, Line b)
{
    if ((crossProduct(b.S-a.F, a.S-a.F) * crossProduct(b.F-a.F, a.S-a.F)) > 0) return false;
    if ((crossProduct(a.F-b.F, b.S-b.F) * crossProduct(a.S-b.F, b.S-b.F)) > 0) return false;
    return true;
}
Point computeLineIntersection(Line a, Line b)
{
    a.S = a.S-a.F; b.S = b.F-b.S; b.F = b.F-a.F;
    if (dotProduct(a.S, a.S) < EPS) return a.F;
    if (dotProduct(b.S, b.S) < EPS) return b.F;
    return a.F + a.S*crossProduct(b.F, b.S)/crossProduct(a.S, b.S);
}
// Point in possibly non-convex polygon, returns 1 for strictly interior points,
// 0 for strictly exterior points, and 0 or 1 for the remaining points
bool pointInPolygon(const Polygon &poly, Point pt)
{
    bool res = false;
    for (int i = 0; i < poly.size(); ++i)
    {
        int j = (i+1) % poly.size();
        if ((poly[i].y <= pt.y && pt.y < poly[j].y || poly[j].y <= pt.x && pt.y < poly[i].y) &&
            pt.x < poly[i].x + (poly[j].x - poly[i].x)*(pt.y - poly[i].y)/(poly[j].y - poly[i].y))
            res = !res;
    }
    return res;
}
bool pointOnPolygon(const Polygon &poly, Point pt)
{
    for (int i = 0; i < poly.size(); ++i)
    {
        Line ln(poly[i], poly[(i+1)%poly.size()]);
        if (sqrDist(projectPointOnLineSegment(ln, pt), pt) < EPS)
            return true;
    }
    return false;
}
vector<Point> circleLineIntersection(Line line, Point center, double radius)
{
    vector<Point> res;
    Point d = line.S-line.F;
    double D = crossProduct(line.F-center, line.S-center);
    double e = sqrt(radius*radius*dotProduct(d, d) - D*D);
    res.push_back (center + Point(D*d.y+(d.y >= 0 ? 1 : -1)*d.x*e, -D*d.x+fabs(d.y)*e) / dotProduct(d,d));
    if (e > 0) res.push_back (center + Point(D*d.y-(d.y >= 0 ? 1 : -1)*d.x*e,-D*d.x-fabs(d.y)*e) / dotProduct(d,d));
    return res;
}
vector<Point> intersectionPointBetweenTwoCircles(Point a, Point b, double r, double R)
{
    vector<Point> ret;
    double d = sqrt(sqrDist(a,b));
    if (d > r+R || d+min(r,R) < max(r,R)) return ret;
    double x = (d*d-R*R+r*r)/(2*d);
    double y = sqrt(r*r-x*x);
    Point v = (b-a)/d;
    ret.push_back (a+v*x + rotateCounterClockwise(v, M_PI/2)*y);
    if (y > 0) ret.push_back (a+v*x - rotateCounterClockwise(v, M_PI/2)*y);
    return ret;
}
double areaBetweenTwoCircles(Point a, Point b, double r, double R)
{
    double n = sqrt((a.x-b.x)*(a.x-b.x) + (a.y-b.y)*(a.y-b.y));
    if (n >= r+R) return 0;
    else if (n+r <= R || n+R <= r) return (M_PI * min(r, R) * min(r, R));
    else
    {
        double a = acos((R*R + n*n - r*r) / (2*R*n)) * 2;
        double b = acos((r*r + n*n - R*R) / (2*r*n)) * 2;
        return ((R*R*a + r*r*b - R*R*sin(a) - r*r*sin(b)) * 0.5);
    }
}
// assuming that the coordinates are listed in a clockwise or counterclockwise order
double polygonSignedArea(Polygon poly)
{
    double area = 0;
    for (int i = 0; i < poly.size(); ++i)
    {
        int j = (i+1) % poly.size();
        area += poly[i].x*poly[j].y - poly[j].x*poly[i].y;
    }
    return area / 2.0;
}
double polygonArea(Polygon poly) { return fabs(polygonSignedArea(poly)); }
pair<double, double> polygonCentroid(Polygon poly)
{
    double cx = 0, cy = 0;
    double scale = 6.0 * polygonSignedArea(poly);
    for (int i = 0; i < poly.size(); ++i)
    {
        int j = (i+1) % poly.size();
        cx += (poly[i].x+poly[j].x) * (poly[i].x*poly[j].y - poly[j].x*poly[i].y);
        cy += (poly[i].y+poly[j].y) * (poly[i].x*poly[j].y - poly[j].x*poly[i].y);
    }
    return {cx/scale, cy/scale};
}
/*  Given a n side polygon (Not necessarily convex) and m lines, it finds section length
    in nmlogn time. If the line fully lies inside the polygon it's that lines length, its
    length that lies inside polygon. */
vector<double> polygonLineIntersection(Polygon poly, vector<Line> lines)
{
    int n = poly.size(), m = lines.size(), x = 0;
    poly.push_back(poly[0]);
    vector<double> ans(m, 0);
    for (auto &line : lines)
    {
        Point a = line.first, b = line.second;
        vec.clear();
        for (int i = 0; i < n; ++i)
            getSegmentIntersection(a, b-a, poly[i], poly[i+1] - poly[i]);
        sort(all(vec));
        double k = 0;
        for (size_t i = 0; i < vec.size(); ++i)
        {
            k += vec[i].second;
            if (dcmp(k)) ans[x] += vec[i+1].first - vec[i].first;
        }
        ans[x++] *= length(b-a);
    }
    return ans;
}
```

```c++
// Rectangles Overlap/Intersection
bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2)
{
    int x1 = rec1[0], y1 = rec1[1], x2 = rec1[2], y2 = rec1[3];
    int x3 = rec2[0], y3 = rec2[1], x4 = rec2[2], y4 = rec2[3];
    // using pos
    return !(x2 <= x3 || y2 <= y3 || x1 >= x4 || y1 >= y4);
    // using area
    return (min(x2, x4) > max(x3, x1)) && (min(y2, y4) > max(y3, y1));
}
// Circle and Rectangle Overlap/Intersection
bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2)
{
    int rectX = x1, rectY = y1, rectWidth = abs(x2-x1), rectHeight = abs(y2-y1);
                   // (These max terms are closest x and y points to circle from rectangle
    int deltaX = x_center - max(rectX, min(x_center, rectX + rectWidth));
    int deltaY = y_center - max(rectY, min(y_center, rectY + rectHeight));
    return (deltaX*deltaX + deltaY*deltaY) <= (radius*radius);
}
```

### [Check Straight Line](https://leetcode.com/problems/check-if-it-is-a-straight-line/)
```c++
// Check slope of continuous segments
bool checkStraightLine(vector<vector<int>>& coordinates)
{
    int p = (coordinates[1][1] - coordinates[0][1]);
    int q = (coordinates[1][0] - coordinates[0][0]);
    for (int i = 2; i < coordinates.size(); ++i)        
    {
        int r = (coordinates[i][1] - coordinates[i-1][1]);
        int s = (coordinates[i][0] - coordinates[i-1][0]);
        if (p * s != r * q) return false;
    }
    return true;
}

// Check three points there triangle area must be zero using heron's formula
https://leetcode.com/problems/check-if-it-is-a-straight-line/discuss/408968/Check-collinearity
```
